# Typora+PicGO+Gitee配置图床超详细

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

### 下载picgo码云插件

![image-20200824155157339](https://gitee.com/szimo/picture_repository/raw/master/images/20200824155157.png)

### 设置picgo的gitee配置参数

![image-20200824155626162](https://gitee.com/szimo/picture_repository/raw/master/images/20200824155626.png)



> repo：仓库地址去掉码云域名
>
> 例如：https://gitee.com/XXXXX/images
> 写成：XXXXX/images
> branch：选择分支 默认为master
>
> token：上面设置的是私人令牌
> path：是仓库中新建images文件夹
> 点击设为默认图床，点击确定



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