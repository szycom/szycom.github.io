* 修改qcow镜像内容

```bash
# 将qcow转成原始镜像
qemu-img convert -f qcow2 -O raw xxx.qcow2 xxx.img

# 查看要挂载点的偏移
SECTORS=`fdisk -lu xxx.img | grep "xxx3" | awk '{print $2}'`
OFFSET=$((${SECTORS * 512}))

# 将img文件仿真成一个设备
LOOP=`losetup -f`
losetup ${LOOP} xxx.img -o ${OFFSET}

# 挂载设备到服务器
mount ${LOOP} xxx_dir
# 通过xxx_dir访问img中内容

# 取消挂载点
unmount xxx_dir
#删除仿真设备
losetup -d ${LOOP}

# 将原始镜像打包成qcow2格式
qemu-img convert -f raw -c -O qcow2 xxx.img xxx.qcow2

```


* 使用virsh创建虚机

```bash
#!/bin/bash
# 更新redis_xml和codis-modify.iso
# $1 gruop id
# $2 slot id

if [ $# -lt 2 ]
then
	echo "Usage:$0 <group-id> <slot-id>"
	exit 1
fi

group=$1
slot=$2

# mac地址增1
function mac_increase(){
        echo "$1" | awk -F ":" 'BEGIN{hex=256}
        function dtoh(i){return strtonum("0x"i)}
        {
                mac=dtoh($1)*hex^5+dtoh($2)*hex^4+dtoh($3)*hex^3+dtoh($4)*hex^2+dtoh($5)*hex+dtoh($6);
                mac++;
                printf("MAC=%02x:%02x:%02x:%02x:%02x:%02x\n",
                int(mac/hex^5),
                int(mac%hex^5/hex^4),
                int(mac%hex^5%hex^4/hex^3),
                int(mac%hex^5%hex^4%hex^3/hex^2),
                int(mac%hex^5%hex^4%hex^3%hex^2/hex),
                mac%hex^5%hex^4%hex^3%hex^2%hex) > "temp_mac"
        }'
}

BASE_DIR=$(readlink -f ${BASH_SOURCE})
BASE_DIR=${BASE_DIR%/*}
MAC_TEMP=${BASE_DIR}/temp_mac
REDIS_XML=${BASE_DIR}/redis.xml
HOST_NAME=$(echo codis-${group}-${slot})
MODIFY_FILE=${BASE_DIR}/redis-modify/openstack/latest/user_data
UUID=$(echo `uuidgen ${HOST_NAME=}`)
ORIGIN_MAC=$(echo `grep '<mac address=' ${REDIS_XML}  | awk -F = '{print $2}'  | awk -F '/>' '{print $1}'`)

mac_increase ${ORIGIN_MAC}

MAC=$(echo `awk -F = '{print $2}' ${MAC_TEMP}`)

# 修改modify文件
sed -i "s,<slot.*slot>,<slot id=\"1\">${slot}</slot>",g ${MODIFY_FILE}
sed -i "s,<group.*group>,<group id=\"1\">${group}</group>",g ${MODIFY_FILE}

# 修改redis.xml文件
sed -i "s,<name.*name>,<name>${HOST_NAME}</name>,g" ${REDIS_XML}
sed -i "s,<uuid.*uuid>,<uuid>${UUID}</uuid>,g" ${REDIS_XML}
sed -i "s,<source file=.*qco'/>,<source file=\'${BASE_DIR}/${HOST_NAME}.qco\'/>,g" ${REDIS_XML}
sed -i "s,<mac address=.*/>,<mac address=\'${MAC}\'/>,g" ${REDIS_XML}


# 生成codis-modify.iso
rm -rf ${BASE_DIR}/codis-modify.iso
mkisofs -o ${BASE_DIR}/codis-modify.iso -R -J -v -T ${BASE_DIR}/redis-modify

rm -rf ./temp_mac

cp ${BASE_DIR}/codis-min-base.qco ${BASE_DIR}/${HOST_NAME}.qco

virsh define ${REDIS_XML}
virsh start ${HOST_NAME}

exit 0 
```

