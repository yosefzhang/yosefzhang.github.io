# 个人笔记系统搭建


# 个人笔记系统搭建

## 笔记系统的选择

以前也用过Notion、OneNote等笔记系统，但是有两个担心，为了本地维护和避免收费，最终选择了免费开源的Joplin。然而Joplin的编辑器不是很好，不过好在它支持外部编辑器，因此选择了好评的Typora。虽然Typora收费了，我觉得能理解。

官网地址：[Joplin (joplinapp.org)](https://joplinapp.org/cn/)

## Joplin的使用

### 修改笔记存储位置

这一步要放在最前面，笔记的存储和配置都会因为修改了位置而变化，否则打开Joplin会恢复成默认的配置，包括设置、CSS样式表等等。

在Joplin.exe的快捷方式上右击，选择属性，再选择快捷方式选项卡，在`目标`一栏保留当前值不变，追加` --profile &#34;D:\001_个人文件\MyJoplin&#34;`，其中`&#34;D:\001_个人文件\MyJoplin&#34;`就是新的笔记存储位置。保存并使用此修改后的快捷方式打开Joplin即可。

![image-20230628114801342](https://aliyun-zhangtao.oss-cn-shanghai.aliyuncs.com/image/image-20230628114801342.png)

### 修改语言

打开Joplin，进入`工具→选项→常规→语言`修改语言。默认是英文，自己对照找到路径设置即可。

### 修改Typora为外部编辑器

1. 安装Typora
2. 打开Joplin，进入`工具→选项→常规`，修改`文本编辑器命令`为你的Typora程序绝对路径。修改`参数`为`-n`。

### 修改Joplin主题

这是很繁琐的过程，网上已有的主题确实好看，但总有一些地方自己看着不爽，奈何CSS没玩过，能修改的不多，此处暂且记录修改部分。找了许久有两个主题较好，推荐使用。

#### Ohmine Dark Theme

也是目前正在用的主题，Github上下载的。根据自己的喜好修改了一些配置，尤其是字体，它推荐的字体中文显示不好看。下载链接：[Ohmine Dark Theme](https://github.com/Nacandev/Ohmine-Dark-Theme-For-Joplin#latest-released)

主题修改步骤：

1. 打开Joplin，进入`工具→选项→外观→显示高级选项`

2. `适用于已渲染Markdown的自定义样式表`是指userstyle.css，用于配置Markdown的编辑区、查看区的样式表。

   `适用于Joplin全域应用样式的自定义样式表`是指userchrome.css，用于配置Joplin应用程序样式表。

3. 分别复制Ohmine DT中的userstyle.css和userchrome.css文件内容到对应文件中，保存并重启Joplin。

4. 根据Ohmine DT中的README.md文件介绍，依次下载插件、配置。

#### 修改字体

我不喜欢Ohmine的推荐字体，因此重新找了字体。

| 应用目标                       | 字体                |
| ------------------------------ | ------------------- |
| 英文字体                       | Times New Roman     |
| 中文字体                       | Source Han Serif SC |
| 等宽字体（适用于表格和代码块） | Fira Code           |

Times New Roman和SimSun基本都默认支持，Fira Code和Source Han Serif SC需要下载。并且我调整了字体的大小。需要注意的是，要同时安装同一字体的不同着重，否则就会出现CSS渲染要加粗字体时，发现仍旧显示常规字体，原因就是没有安装这一字体的加粗字体文件。

修改userstyle.css文件，如下：

```css
  /* GENERAL *********************************************** */
  /* base font group:  all elements that other than monospace font group */
  --base-font: &#34;Times New Roman&#34;, &#34;Source Han Serif SC&#34;; /* by default, it&#39;s also affects to --print-base-font */
  --base-font-size: 16px;

  /* monospace font group:  code block | pre | code | footnote ref no. */
  --monospace-font: &#34;Fira Code&#34;, monospace, &#34;Source Han Serif SC&#34;; /* by default, it&#39;s also affects to --print-monospace-font */
  --monospace-font-size: 12px;
```

#### 修改行内代码段样式

修改userstyle.css文件，如下：

```css
/* special text: inline code text -------------------------- */
#rendered-md .inline-code,
.mce-content-body code:not(.joplin-editable &gt; pre &gt; code) {
  color: var(--inline-code-text-color);
  padding: 0 3px;
  background: var(--inline-code-background-color);
  box-shadow: 0 0 1px var(--inline-code-border-color);
  text-shadow: 1px 1px 2px var(--inline-code-text-shadow);
  font-family: &#34;Times New Roman&#34;;
  font-size: 16px;
}
```

### 其他修改

我把渲染背景色修改成护眼绿了，导致其他的都需要修改配色，研究了一下午的配色和CSS代码，终于是搞定了一套看起来比较舒服的配色。

![image-20230629180059018](https://aliyun-zhangtao.oss-cn-shanghai.aliyuncs.com/image/image-20230629180059018.png)

### CSS中引用图片

在修改CSS渲染时，我想实现将目录前增加一个特殊好看的图案作为列表的部分，这里就会用到list-style-image。实际的过程却是很曲折，我发现和userstyle.css文件在同一目录下的图片无法被直接引用。偶然间查看到一篇文档，大概意思是joplin会将userstyle.css文件打开后存放到tmp目录，也就是说，我如果要引用文件，应该先从tmp目录中出来，再去引用。

```css
userstyle.css：
……
list-style-image: url(&#34;../toc.png&#34;);/* Correct */
list-style-image: url(toc.png&#34;);/* Wrong */
……
```

## Typora的使用

最终选择使用Typora做为“打字机”，付了85 RMB。

### Typora插件

- PicGo：参考图床搭建。
- Pandoc：文档转化的“瑞士军刀”。下载地址：[Releases · Molunerfinn/PicGo (github.com)](https://github.com/Molunerfinn/PicGo/releases)

### 问题记录

#### Ctrl&#43;2无法打出二级标题

其它级别标题都是正常的，但是用Ctrl&#43;2无法打出二级标题，这个问题没搜到解决方案，索性将标题快捷键全部替换掉。

参考链接：[Shortcut Keys - Typora Support (typoraio.cn)](https://support.typoraio.cn/Shortcut-Keys/#change-shortcut-keys)

步骤如下：

1. 打开Typora，进入`文件→偏好设置→通用→打开高级设置`
2. 弹出文件夹中，打开**conf.user.json**
3. 在**keyBinding**中插入以下代码

```json {.line-numbers}
  &#34;keyBinding&#34;: {
    // for example: 
    // &#34;Always on Top&#34;: &#34;Ctrl&#43;Shift&#43;P&#34;
    // All other options are the menu items &#39;text label&#39; displayed from each typora menu
    &#34;Heading 1&#34;: &#34;Alt&#43;1&#34;
    &#34;Heading 2&#34;: &#34;Alt&#43;2&#34;
    &#34;Heading 3&#34;: &#34;Alt&#43;3&#34;
    &#34;Heading 4&#34;: &#34;Alt&#43;4&#34;
    &#34;Heading 5&#34;: &#34;Alt&#43;5&#34;
    &#34;Heading 6&#34;: &#34;Alt&#43;6&#34;
  },
```

4. 保存并重启Typora。

## 图床搭建

Typora &#43; PicGo &#43; 阿里云OSS

### 阿里云OSS

我的OSS：[OSS管理控制台 (aliyun.com)](https://oss.console.aliyun.com/overview)

购买了40GB的标准存储包，2年有效期，共计花费18RMB，挺划算的。

但是！！这40GB是指**存储**的容量大小，别人访问下载是需要额外付费的，好在这个费用很低很低。

#### 创建Bucket

按照OSS的指导，创建了一个用于图床的Bucket，命名为**aliyun-zhangtao**，设置Bucket ACL为**公共读**。

#### 创建仅访问用户

创建该Bucket的用户，设置对应的权限。注意在创建用户设置权限时，下述的Accesskey Secret要保存好，页面退出后很难找回。

| 用户登录名称     | access@1516887916714135.onaliyun.com                         |
| ---------------- | ------------------------------------------------------------ |
| 登录密码         |                                                              |
| AccessKey ID     | LTAI5tH32PDMjcoqA7LqnGow                                     |
| AccessKey Secret | ********************************************************************* |

### PicGo

下载PicGo：[PicGo](https://picgo.github.io/PicGo-Doc/zh/)

配置图床链接到阿里云OSS：

|           |                                                              |
| --------- | ------------------------------------------------------------ |
| KeyId     | LTAI5tH32PDMjcoqA7LqnGow                                     |
| KeySecret | ********************************************************************* |
| Bucket    | aliyun-zhangtao                                              |
| 存储区域  | oss-cn-shanghai                                              |
| 存储路径  | image/                                                       |

![image-20230706190806992](https://aliyun-zhangtao.oss-cn-shanghai.aliyuncs.com/image/image-20230706190806992.png)

### Typora图床配置

![image-20230706190923021](https://aliyun-zhangtao.oss-cn-shanghai.aliyuncs.com/image/image-20230706190923021.png)

### 图床验证

当在Typora写文档时，复制图片进文档里时，PicGo会自动将该图片上传到阿里云OSS对应的Bucket中，然后将文档中的图片链接修改为阿里云OSS的图片链接。这样当该文档在任意电脑打开时，其图片资源访问的是阿里云OSS的图床。

## 笔记多端同步

### 同步方案思考

查阅资料后发现很多同步Joplin笔记的方法，最多介绍的就是坚果云和Dropbox，还有就是OneDriver。我尝试了OneDriver，然而速度感人，弃用之。

突然想到我在搭建图床时购买了阿里云OSS的服务，这或许也可以当做一个同步笔记的方案。搜索资料，还真的发现了一篇文章：[Joplin与阿里云OSS做同步 - xiaostudy - 博客园 (cnblogs.com)](https://www.cnblogs.com/xiaostudy/p/16111714.html)。经过我的验证，可行！需要注意的是，这会产生较多的PUT和GET类型请求，不过收费还是很低的。关于阿里云OSS，我准备单独写一篇介绍文档。

### 阿里云OSS设置

为了区别于图床，新建了一个Bucket：**zhangtao-joplin**，用于同步笔记。新增了访问账户如下：

| 用户登录名称     | joplin@1516887916714135.onaliyun.com                         |
| ---------------- | ------------------------------------------------------------ |
| 登录密码         |                                                              |
| AccessKey ID     | LTAI5tET63YpMq8Q5XzRb9UE                                     |
| AccessKey Secret | ************************************************************* |

### Joplin同步配置

如下图所示，填入对应的参数即可。尝试了下，还是比较稳定的，速度比OneDriver快多了。不过随着笔记越来越大，附件越来越多的情况下，存储的空间也会占用的更大。另外，建议把同步间隔设置的大一些，以免出发较多的PUT和GET请求。

![image-20230706191731094](https://aliyun-zhangtao.oss-cn-shanghai.aliyuncs.com/image/image-20230706191731094.png)


---

> 作者: 易相  
> URL: https://blog.yosefzhang.top/posts/%E4%B8%AA%E4%BA%BA%E7%AC%94%E8%AE%B0%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA/  

