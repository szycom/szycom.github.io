## Windows 设置hosts

> 手动指定域名和IP的映射关系

* **hosts文件位置**

```html
C:\windows\system32\drivers\etc
```

* **刷新DNS内容**

```html
win+r，输入CMD，回车

在命令行执行:ipconfig /flushdns     #清除DNS缓存内容。
```

* **显示DNS缓存内容**

```html
ipconfig /displaydns    //显示DNS缓存内容
```

## Linux设置hosts

* **文件位置**

```html
/etc/hosts
```

* **刷新DNS内容**

```html
systemctl restart nscd
```
