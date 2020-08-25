# Typora+PicGO+Gitee配置图床

### 工具

- Typora 0.9.84版本以上
- PicGo 2.2.0版本以及以上
- Gitee

本文版本信息：*Typora 0.9.93* *GICGo 2.2.2*

### 设置gitee仓库

1. 新建一个**公共仓库**
2. 新建一个资源文件夹`images`（名字可任意）

3. 设置个人token



- 点击头像，选择设置

![image-20200824135419307](https://gitee.com/szimo/picture_repository/raw/master/images/20200824135419.png)



- 左侧选择私人令牌

![image-20200824135602972](https://gitee.com/szimo/picture_repository/raw/master/images/20200824135603.png)

- 私人令牌设置以后只可以查看一次，记得保存

![image-20200824155051130](https://gitee.com/szimo/picture_repository/raw/master/images/20200824155051.png)

### 下载picgo

下载地址：https://github.com/Molunerfinn/PicGo/tags

![](https://gitee.com/szimo/picture_repository/raw/master/images/20200825193710.png)

### 下载插件

![image-20200824155157339](https://gitee.com/szimo/picture_repository/raw/master/images/20200824155157.png)

### 设置picgo的gitee配置参数

![image-20200824155626162](https://gitee.com/szimo/picture_repository/raw/master/images/20200824155626.png)



> repo：仓库地址去掉码云域名
>
> 例如：https://gitee.com/szimo/picture_repository
>
> 写成：szimo/picture_repository
>
> branch：选择分支 默认为master
>
> token：上面设置的是私人令牌
>
> path：是仓库中新建images文件夹
>
> **点击设为默认图床，点击确定**



### 在picgo上传区测试上传图片

这时会在你的仓库看见文件已经更新

### typora设置

**如图**

- 文件->偏好设置

![image-20200824134826194](https://gitee.com/szimo/picture_repository/raw/master/images/20200824134826.png)

- 图像中按照下图设置即可



![image-20200824134712526](https://gitee.com/szimo/picture_repository/raw/master/images/20200824134712.png)

- 点击验证图片上传选项，成功如下图

![image-20200824134744648](https://gitee.com/szimo/picture_repository/raw/master/images/20200824134744.png)

### 设置picgo上传图片的快捷键（可选）Ctrl+shift+p


![image-20200824135039528](https://gitee.com/szimo/picture_repository/raw/master/images/20200824135039.png)



### Typora使用

直接在typora中ctrl+v即可完成上传

如果上传失败，就先ctrl+shift+p上传，在ctrl+v复制即可

### 小技巧

可以选择 *格式*`->`*图像*`->`*上传所有本地图片*

![image-20200825202023658](https://gitee.com/szimo/picture_repository/raw/master/images/20200825202023.png)







### 常见问题

#### 错误一：Failed to fetch

![image-20200825201128870](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201128.png)

这个错误一般是由**端口设置错误**造成的（查看日志log文件），打开picgo的log文件。错误提示是端口繁忙。

![image-20200825201250106](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201250.png)

*解决方法*：打开PicGo设置，点击设置Server选项，**将端口改为36677端口**，这是picgo推荐的默认端口号，然后保存。

![image-20200825201353246](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201353.png)

不过有的时候，Failed to fetch还是如约而至，打开端口设置一看，怎么变成了366771或者其他的端口了？？？？

问题在于端口冲突，如果你打开了多个picgo程序，就会端口冲突，**picgo自动帮你把36677端口改为366771端口**，导致错误。log文件里也写得很清楚。

![image-20200825201515624](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201515.png)

#### 错误二：{“success”,false}

![image-20200825201557597](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201557.png)

原因是**文件名冲突**了，如果你上传过一张image1.jpg的图片，再上传名称一样的图片就会失败，看看log文件里也写到了。

![image-20200825201644385](https://gitee.com/szimo/picture_repository/raw/master/images/20200825201644.png)

#### 总结

遇到错误先看日志，方便排查