# 使用purite和Vercel高效简易地部署你的静态博客

如果你希望在5分钟内构建一个静态的博客或文档网站，并且希望专注于写作本身而不是复杂的操作和奇怪的Bug，那么purite也许是个不错的选择。

purite可以使你每当想要写作时，需要做的就只有编写一个Markdown文档并运行电脑上的purite程序，然后呢？purite就会为你做好一切，打开浏览器即可以较高的速度访问你的网站和新文章。

本站便是purite的一个实现。

## 部署站点

你需要先在自己的电脑上安装[Git](https://git-scm.com/)和[Pandoc](https://pandoc.org/)工具并进行相应配置，因为purite的很多执行操作是基于这两者的，这里不赘述其过程。

接下来在[GitHub](https://github,com/)上新建一个仓库，新建时只需要填写仓库名称，可见性选择Public，就像这样：

![新建仓库](https://f-y-blog.oss-cn-shenzhen.aliyuncs.com/0-2.png)

创建完成后能够得到这样一个链接：

![仓库链接](https://f-y-blog.oss-cn-shenzhen.aliyuncs.com/0-3.png)

注意：这个时候要选择HTTPS链接而不是SSH，否则可能在部署时会出现一些问题。将类似https://github.com/icecream1507/purite-example.git的地址复制保存下来，这就是将来的博客仓库地址。请保证你的GitHub账号保存了你正在使用的设备的公钥，也就是说你的设备可以正常进行Push等操作。

接着在[本项目Release页面](https://github.com/icecream1507/purite/releases/tag/v0.0.1)下载适用于你的操作系统的压缩包，解压后的文件夹就是你的博客的本地文件夹。打开后应该长这样:

![解压后的文件夹](https://f-y-blog.oss-cn-shenzhen.aliyuncs.com/0-1.png)

第一个`0`文件夹是默认的第一篇文章的文件夹，第二个`index-template.md`是站点首页的模版文件，第三个`purite(.exe)`是自动进行部署的可执行文件。

现在就让我们看看purite的特别之处。打开`index-template.md`，你可以修改`h1`标题成你想要的站点标题。你应该注意到了有一行`%purite:article_list%`，这便是文章列表变量，你可以把它放在这个文件的任何地方，只要保证它单独成行并没有被Markdown语法修饰，这样在自动生成页面时就会将其替换为文章列表。除了变量外，其他你所看到的文字都是字面内容，可以随意编辑。变量的使用可以使你较为自由地改变页面结构、样式，并且不需要与代码打交道。

![首页模版文件](https://f-y-blog.oss-cn-shenzhen.aliyuncs.com/0-6.png)

`0`文件夹中有一个`hello-world.md`，这是默认的第一篇文章。在将来创作第`n`篇文章时，你也需要像这样创建一个名为`n-1`的文件夹，并在该文件夹中编写唯一的Markdown文件，内容就是你的文章，自动生成时此文件会被转化为HTML页面，该文件最好以英文缩略标题命名（例如`hello-world.md`）以便生成易懂的文章链接，而该文件的第一行需要是一个`h1`标题作为这篇文章的标题（例如这个`hello-world.md`第一行的`h1`标题内容为“你好，世界！”，则此文在文章列表中的标题就是“你好，世界！”）。在这个文件夹中你可以存放与该文章相关的文件，例如要插入的图片、附件等。

了解了本地博客文件夹的基本结构，就可以开始自动构建了。macOS和Linux用户可以打开终端，进入本地博客文件夹，执行：

```shell
sudo ./purite
```

![终端处理界面](https://f-y-blog.oss-cn-shenzhen.aliyuncs.com/0-7.png)

按照提示，依次输入上面创建的Git仓库地址和本次Git提交的备注信息。purite就会帮你初始化Git仓库，把Markdown文件转为HTML页面，生成配置文件，并把内容推送至远程仓库。

Windows用户可以以管理员身份运行`purite.exe`，后面过程一样。

Github的仓库创建完毕后，我们需要将其与[Vercel](https://vercel.com)关联起来。Vercel是一家提供静态页面托管服务的网站，支持从Github等Git仓库拉取数据并构建静态网站，并使用CDN加速其访问，在这里我们就要用到它的静态页面托管功能。

进入Vercel的网站，选择通过Github账号注册并登录，然后就可以导入项目了：

![导入项目1](https://f-y-blog.oss-cn-shenzhen.aliyuncs.com/0-4.png)

先点击左边的`Continue`，再将上面的仓库地址输入并继续。

![导入项目2](https://f-y-blog.oss-cn-shenzhen.aliyuncs.com/0-5.png)

项目名称可以自己取，预设框架选择`Other`，最后两个不用管，点击`Deploy`，Vercel就会从你的仓库拉取代码并自动部署网站：

![项目导入设置](/Users/tedfu/Library/Application Support/typora-user-images/image-20200815005619771.png)

![成功部署](https://f-y-blog.oss-cn-shenzhen.aliyuncs.com/0-9.png)

部署好之后Vercel会给你一个域名，用它就可以访问网站了。Vercel也支持自定义域名，你可以根据它的提示来进一步设置相关内容。

至此，我们的静态博客部署就全部完成了，如果你对于本文中提到的概念都比较熟悉的话，总时长应该没有超过5分钟吧？（如果我打脸了我就再改改，争取简化流程）这样的站点构建方式可以说是相当轻量了，而部署完成后，要如何进行创作呢？

## 创作并发布

非常简单，三步即可搞定：

1. 按上面提到的方式为你要创作的文章创建一个文件夹
2. 按上面提到的方式用Markdown写好文章，丢到这个文件夹里
3. 以管理员身份运行purite可执行文件

做完这三步，如果没有发生错误，那么你的文章就应该发布完成，可以直接在浏览器中查看了。

## 自定义站点

打开`config.json`，里面大概长这样

```json
{
    "GitRepo": "https://github.com/icecream1507/purite-example.git",
    "HTMLHead": "\u003chead\u003e\n\u003clink rel=\"stylesheet\" href=\"https://f-y-blog.oss-cn-shenzhen.aliyuncs.com/style.css\" /\u003e"
}
```

你可以修改`"GitRepo"：`后的内容来更改博客的远程仓库。

`"HTMLHead":`后的内容转换成可读的字符串就是：

```html
<head>
<link rel="stylesheet" href="https://f-y-blog.oss-cn-shenzhen.aliyuncs.com/style.css" />"
```

这其实是一个文本替换的素材，purite在处理生成的HTML页面时会将源码中的`<head>`替换为上面的两行内容，相当于在HTML页面头部增加信息。这里默认是加了一个样式表的引用，来实现一个简单的主题，你也可以改动`"HTMLHead":`后的内容来自定义头部信息。

## 最后

目前purite还在第一个测试版，开发时长为一个通宵。因此，代码不够成熟优雅，功能也比较简单，所以不适合用于生成环境，仅供个人试用。后续我也会抽时间进行开发，不断进行完善。

那么，purite这个名字的意义是什么呢？
$$
purite=pure+write
$$
所以，希望purite能给你带来“纯粹的写作”体验。



***Ted Fu***

***2020年八月15日凌晨于广州***