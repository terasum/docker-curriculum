---
layout: layout.html
---

## 概述简介

### 什么是 Docker?


维基百科将[Docker](https://www.docker.com/)定义为

> an open-source project that automates the deployment of software applications inside **containers** by providing an additional layer of abstraction and automation of **OS-level virtualization** on Linux.

> Docker是一个在Linux操作系统上通过**操作系统级别虚拟化技术**来提供一层额外的自动化抽象层实现将软件应用自动化地部署到容器当中的一个开源项目。

Wow! 好绕啊！ 简单来说，Docker是一个允许开发人员、系统管理员等角色轻松地将他们的应用程序部署在沙箱（称为**容器**）中，以便在宿主操作系统（即Linux）上运行的工具。

Docker的主要优点是，它允许用户**将具有所有应用程序以及其所依赖的包一起打包到用于软件开发的标准化模块单元（镜像）**中。与虚拟机不同的是，容器开销不高，能够更有效地使用底层系统和资源。

### 什么是容器 （containers）？

当今的行业标准是使用虚拟机（VM）来运行软件应用程序。 虚拟机运行客户操作系统(guest operation system), 用户在客户操作系统内运行应用程序。而客户操作系统在由宿主机操作系统(host operation system)支持的虚拟硬件上运行。

虚拟机非常适合为应用程序提供完全的进程隔离，宿主操作系统中的BUG很少会影响客户操作系统中运行的软件，反之亦然。 但是这种隔离成本很高 —— 由于为客户操作系统虚拟化硬件的计算开销很大。

容器技术采用了不同的方法：通过利用宿主操作系统的底层（low-level）的一些虚拟化机制，容器可以以一小部分计算能力提供大部分同虚拟机技术类似的隔离功能。

### 为什么使用容器？

容器提供了一种更加符合逻辑的打包机制，这种机制可以将应用程序从其实际运行的环境中抽象出来。不论目标运行环境是私有的数据中心，公有云还是开发人员的个人笔记本，这种分离打包过程都是一致的，并且是轻松的。因此基于容器的应用程序是环境可预测的，并且是同其它应用程序相隔离的，能够在任意支持Docker的宿主环境运行。

从运维的角度看，除了高效可移植性之外，容器还可以对资源进行更加精细的控制，从而提高基础架构的效率，更好地利用计算资源。


<picture>
  <source type="image/webp" srcset="images/interest.webp">
  <img src="images/interest.png" alt="Docker interest over time">
  <p class="caption">Google Trends for Docker</p>
</picture>

由于上述的这些优点，容器(和Docker)已经被业界广泛词用。 谷歌，Facebook，Netflix和Salesforce等公司利用容器来提高大型工程团队的工作效率，并提高计算资源的利用率。 实际上，谷歌认为容器技术降低了开发对整个数据中心的依赖程度。

### 这个教程将会提供什么?

本教程旨在成为让您轻松掌握Docker的一站式教程。 除了揭开Docker风景的神秘面纱之外，它还将为您提供在云上构建和部署自己的Web应用程序的实践经验。我们将使用[Amazon Web Services](http://aws.amazon.com)在[EC2](https://aws.amazon.com/ec2/)上部署静态网站和两个动态网络应用程序 [Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/)和[Elastic Container Service](https://aws.amazon.com/ecs/)。 即使您之前没有部署经验，本教程也能够帮助您完成全部任务内容。

___________

## 从这里开始

本文档包含一系列的几个部分，每个部分都解释了Docker的一个特定方面。 在每个部分中，我们将输入命令（或编写代码）。 本教程中使用的所有代码都可以在[Github repo](http://github.com/prakhar1989/docker-curriculum)中找到。

> 请注意：本教程使用的是Docker **18.05.0-c2** 版本，如果您在学习过程当中发现由任何本教程和未来的Docker版本不兼容的问题，请随时提出(https://github.com/prakhar1989/docker-curriculum/issues). 非常感谢！

### 预备资源

除了基本的命令行和文本编辑器的使用经验之外，本教程没有任何需要预先掌握的知识。如果之前有进行过Web相关的开发经验将会有所帮助，但不是必需的。为了能够顺利地完成本教程，我们需要使用一些云服务。如果要继续的话，请务必在以下的网站中均创建一个账户：

- [Amazon Web Services](http://aws.amazon.com/)
- [Docker Hub](https://hub.docker.com/)

### Setting up your computer
### 设置您的电脑

在自己的电脑上安装开发工具与环境往往是一项艰巨的任务，但是好在Docker目前是一个非常稳定的产品，让Docker在您喜欢的操作系统上运行已经非常容易。

直到几个版本之前，在OSX和Windows运行Docker是非常麻烦的。但好在，Docker已经投入了大量的资源来改善这个体验，目前在各个操作系统上运行Docker是非常便捷的事情了。 Docker官方提供了简单的**getting started**教程，来指导大家如何在不同的操作系统上完成Docker的安装：[Mac](https://www.docker.com/products/docker#/mac), [Linux](https://www.docker.com/products/docker#/linux) 以及 [Windows](https://www.docker.com/products/docker#/windows).

> 译注： 由于安装教程是英文的，如果您使用的是Linux操作系统，可以使用阿里云一键安装脚本：
> `curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -`
> 
> 如果您使用的是MacOS，您可以到[这里](https://hub.docker.com/editions/community/docker-ce-desktop-mac)下载dmg安装包。
> 

一旦已经完成Docker的安装,你可以终端中输入如下命令来测试你的Docker是否安装正确：


```bash
$ docker run hello-world

Hello from Docker.
This message shows that your installation appears to be working correctly.
...
```

___________

## Hello World

### 玩一玩 Busybox

现在为止我们已经准备妥当，是时候体验一下Docker了。在这部分，我们将会体验一下如何运行一个[Busybox]()镜像，并体验一下`docker run`命令。

简单开始，我们需要先在终端中运行如下命令：

```bash
$ docker pull busybox
```


> 注意： 取决于你是如何在自己的电脑中安装Docker的，在运行上述命令之后，有可能会出现 `permission denied` 的错误。 如果你使用的是Mac电脑，首先确保你的Docker引擎应用已经运行。如果你在Linux上，该问题一般是用户权限不足的原因找出的，你可以在`docker`命令最前面加上`sudo`来运行，或者将当前用户置于`docker`用户组中：`sudo usermod -aG docker yourusername`，并重新登录即可。

`pull`命令将会从[**Docker仓库**](https://hub.docker.com/explore/)拉取busybox的[**镜像**](https://hub.docker.com/_/busybox/)并保存到你的本地计算机。你可以使用`docker images`命令查看当前在你的操作系统中存在的所有镜像。

```bash
$ docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
busybox                 latest              c51f86c28340        4 weeks ago         1.109 MB
```

### Docker Run

完美！现在让我们试试如何运行一个基于busybox镜像的Docker容器。为了完成这件事我们需要使用`docker run`命令。

```bash
$ docker run busybox
$
```

等等，什么都没有发生！这是bug吗？emm，当然不是。在这个场景背后，发生了很多事情，当你运行了`run`命令，Docker客户端将会首先寻找一个镜像(在这个例子中是 busybox)，然后通过镜像将容器加载并运行。当我们运行`docker run busybox`的时候，我们没有为该容器提供我们需要运行的实际命令，因此这个容器启动之后，运行了一个空命令就退出了。好吧，真的是有一点点绕，那么让我们试试如何尝试一些更加激动人心的事情。

```bash
$ docker run busybox echo "hello from busybox"
hello from busybox
```

Nice - 我们终于看到了一些输出。在这个例子中，Docker 客户端忠实地在busybox容器中执行了`echo`命令，然后退出了，如果你观察仔细的话，所有的动作运行得非常快。想象一下运行一个虚拟机并运行一条简单命令然后再关闭它需要花费多少时间。所以现在你知道容器是多么的快了吧。好啦！让我们看看`docker ps`命令能做什么。`docker ps`命令可以显示当前在你的系统当中所有正在运行的容器。

```bash
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

由于目前没有容器正在运行（之前的运行过的容器都退出了），我们只能看到空白的结果。让我们试试更加有用的命令参数 `docker ps -a`

```bash
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
305297d7a235        busybox             "uptime"            11 minutes ago      Exited (0) 11 minutes ago                       distracted_goldstine
ff0a5c3750b9        busybox             "sh"                12 minutes ago      Exited (0) 12 minutes ago                       elated_ramanujan
14e5bd11d164        hello-world         "/hello"            2 minutes ago       Exited (0) 2 minutes ago                        thirsty_euclid
```

我们看到命令列出了所有我们运行过的容器。格外注意一下`STATUS`列中显示的容器目前的状态，可以看到这些容器在几分钟之前才退出。

你一定想知道怎么样才能让容器一直运行而不是直接退出，让我们试试下面的命令

```bash
$ docker run -it busybox sh
/ # ls
bin   dev   etc   home  proc  root  sys   tmp   usr   var
/ # uptime
 05:45:21 up  5:58,  0 users,  load average: 0.00, 0.01, 0.04
```

运行`run`并加上`-it`标志表示运行容器后，进入交互式命令行模式。进入交互式命令行之后我们可以在容器中运行任意多的我们希望运行的命令，你可以花上一些时间试试你最喜欢的命令。

> **危险操作**： 如果你突发奇想想在容器中试试`rm -rf bin`，请确保这条命令是在容器中而不是在你的实际笔记本和桌面电脑上运行，如果你做了这件事，你讲无法使用类似于`ls`、`echo`这样的命令，但是你可以退出容器，通过`exit`回车可以退出，当你重新用`docer run -it busybox sh`命令启动一个新的容器之后，由于Docker每次都会创建一个新的容器，所以所有的状态将恢复如旧。

这是一个很简单的使用了`docker run`的教程，但这条命令将会是你最为常用的命令之一。你可能需要花点时间去适应它。如果你想知道更多有关`run`命令的细节，你可以使用`docker run --help`得到一个帮助列表，在后面的教程当中，我们将会看到更多的`docker run`命令选项。

在我们继续之前，我们先看看如何删除一个容器，我们看到即使我们退出了容器，我们依旧还是能够通过`docker ps -a`看到容器依旧存在着。在这个教程当中，我们将会运行很多次`docker run`命令，然而容器的存在将会消耗存储空间，也因此我们可以删掉一些不必要的容器，通过`docker rm`命令，然后拷贝容器ID即可，具体命令如下：

```bash
$ docker rm 305297d7a235 ff0a5c3750b9
305297d7a235
ff0a5c3750b9
```

当删除成功，你讲看到被删除的容器ID被输出，如果你有很多容器需要删除，又不想一次一次赋值ID那么你可以通过如下命令完成一次性删除:

```bash
$ docker rm $(docker ps -a -q -f status=exited)
```

这条命令将会删除所有状态为`exited`的容器，`-q`选项表示将会只返回容器16进制容器ID, `-f`表示输出将会基于后面的过滤条件。最后我们捎带提一下`--rm`，在`docker run`之时加入`--rm`选项，当该容器退出之后，容器将会自动删除，因此`--rm`选项是非常有用的。

在最新版本的 Docker中， `docker container prune` 命令将会得到同上述`docker rm ...`相同的效果


```bash
$ docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
4a7f7eebae0f63178aff7eb0aa39f0627a203ab2df258c1a00b456cf20063
f98f9c2aa1eaf727e4ec9c0283bcaa4762fbdba7f26191f26c97f64090360

Total reclaimed space: 212 B
```

最后的最后，如果你想删除一个不想要的镜像，你可以通过`docker rmi <imageid>` 完成。


### 术语

在这一节中我们介绍了很多Docker相关的专业术语，它可能让大家困惑。因此在我们继续之前，我们需要澄清一下这些术语的含义，因为这些术语在Docker生态中被大量使用。

- *Images* - 镜像，我没让你放入应用的基础蓝图，所有的容器都基于镜像为基础运行，在前面的`docker pull`命名就是一个拉取镜像的过程。

- *Containers* - 容器，是从镜像中派生出来的应用实例，可以理解为镜像是一个“类”，而容器是一个“对象”。我们可以通过`docker run`创建一个容器，也可以通过`docker ps`列出当前正在运行的容器实例列表

- *Docker Daemon* - Docker 守护进程，在宿主机上运行的，支持Docker功能的守护进程，所有的Docker容器进程都通过该服务来和宿主环境进行通信。

- *Docker Client* - Docker客户端，是一个允许用户同Docker Daemon 交互的命令行工具. 当然还有除了官方工具之外的客户端工具，如[Kitematic](https://kitematic.com/),它提供了用户界面方面用户操作Docker。

- *Docker Hub* - 一个Docker镜像仓库[registry](https://hub.docker.com/explore/)。你可以把镜像仓库理解为是镜像的文件夹，如果需要的话，你可以从这个地方找到对应的镜像完成你的工作。


-------

## Docker 上的 Webapp

非常好！我们目前已经学会如何使用`docker run`命令了，并且我们还解释了一些比较重要的Docker生态术语。既然已经学会了这些，那么我们可以看看一些实际的玩意儿了，比如，如何通过Docker部署一个web 应用!

### 静态站点

让我们先从简单的开始。首先我们先学习一下如何运行一个非常简单的静态网站。我们首先从Docker Hub上拉取一个镜像下来，然后我们看看通过容器运行一个webserver是多么地简单。

那我们就开始吧，我们将会使用一个简单的[单页网站](http://github.com/prakhar1989/docker-curriculum) , 这是我用来演示这个例子所创建的一个简单的demo，并且将这个demo运行在了 [registry](https://hub.docker.com/r/prakhar1989/static-site/) - `prakhar1989/static-site` 上面.我们可以下载并且通过`docker run`直接运行这个景象，当然我们在前面也提到过，用过为命令添加`--rm`选项，可以自动地将停止的容器删除。

```bash
$ docker run --rm prakhar1989/static-site
```

由于镜像在本地并不存在，客户端将首先将镜像从仓库拉取下来，然后再运行镜像。如果一切正常，那么你将会看到 `Nginx is runing...`的信息显示在终端上。好了现在你的服务已经正常运行了，应该怎么去查看是否正常工作了呢？我们的网站又运行监听在哪个端口呢，如何大概网站，以及最重要的，我们应该如何通过我们的宿主机直接访问正在运行的容器呢？别着急，我们先按 Ctrl+C结束这个容器。

好吧，其实在上面的例子当中我们没有开放任何端口，为了完成这个任务我们需要在`docker run`命令的时候同时开放这些端口。当然我们需要完成这件事情，为了实现这个功能我们需要在运行容器的时候，不进入其交互式命令界面（不然的话我们就不能输入新的命令了，它将会一直都显示`Nginx is runing`）。通过这种方式，你可以在保持容器继续运行的同时，也能够继续使用你的终端，这种方式也成为**detached**模式。

```bash
$ docker run -d -P --name static-site prakhar1989/static-site
e61d12292d69556eabe2a44c16cbd54486b2527e2ce4f95438e504afb7b02810
```

在上面的命令中，`-d`选项表示将释放我们的终端，以免附着到容器进程中，`-P`将会开放所有已经声明需要开放的端口，并为这些容器声明开放的端口分配一个随机的宿主机端口，最后`--name`表示我们需要为容器起的名字。现在我们可以通过命令`docker port [CONTAINER]`看到我们的容器运行在哪个端口：

```bash
$ docker port static-site
80/tcp -> 0.0.0.0:32769
443/tcp -> 0.0.0.0:32768
```

现在你可以在浏览器中打开 [http://localhost:32769](http://localhost:32769) 查看静态网站

> 注意：如果你使用的是 docker-toolbox， 你可能需要使用 `docker-machine ip default` 取得实际IP

你当然可以通过以下命令直接指定需要开放的端口，客户端将会把所有的连接转发到容器当中:

```bash
$ docker run -p 8888:80 prakhar1989/static-site
Nginx is running...
```

<picture>
  <source type="image/webp" srcset="images/static.webp">
  <img src="images/static.png" alt="static site">
</picture>

可以使用`docker stop [CONTAINER ID/NAME]`来停止一个非附着容器，在这个例子当中，我们也可以用容器名来停止这个容器:

```bash
$ docker stop static-site
static-site
```

我相信你一定也觉得这个过程是十分简单的。如果要把这个网站部署到一个真实的服务器上，其实你只需要安装好Docker然后再运行上述命令即可。你一定想知道我是如何在Docker image中创建一个webserver的，这部分内容将在下一节中叙述：

### Docker Images

在前面我们已经介绍了Docker镜像的相关内容，但是在这一节当中我们需要深入地理解一下Docker 镜像是如何构建的。最后我们将把我们自己构建的应用镜像部署到[AWS](http://aws.amazon.com) ，这样我们就可以把我们第一个应用分享给我们的朋友啦，是不是很棒！让我们开始吧！

Docker 镜像是容器的基础，在前面的例子当中，我们从镜像仓库**拉取**了*busybox*，然后请求Docker客户端基于这个镜像运行一个容器。我们可以通过`docker images`命令来看有哪些镜像是可用的：

```bash
$ docker images
REPOSITORY                      TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
prakhar1989/catnip              latest              c7ffb5626a50        2 hours ago         697.9 MB
prakhar1989/static-site         latest              b270625a1631        21 hours ago        133.9 MB
python                          3-onbuild           cf4002b2c383        5 days ago          688.8 MB
martin/docker-cleanup-volumes   latest              b42990daaca2        7 weeks ago         22.14 MB
ubuntu                          latest              e9ae3c220b23        7 weeks ago         187.9 MB
busybox                         latest              c51f86c28340        9 weeks ago         1.109 MB
hello-world                     latest              0a6ba66e537a        11 weeks ago        960 B
```

上面列出了当前我们从仓库中拉取的所有的镜像列表，当然还包括了一个由我自己创建的镜像（很快我们将知道如何创建一个镜像）。其中`TAG`表示当前的这个镜像的快照版本（同一个镜像可以有很多快照），而`IMAGE ID`则是对应的唯一的镜像标识符。

简单地说，你可以把镜像理解为是一个git仓库——镜像被修改之后可以被[提交 committed](https://docs.docker.com/engine/reference/commandline/commit/)，当然它也会有很多版本，也就是前面说的快照版本。如果你没有制定一个特定的版本号，Docker客户端会帮你打上一个`latest`的版本。例如，拉取一个特定版本的`ubuntu`镜像:

```bash
$ docker pull ubuntu:12.04
```

为了得到一个新的Docker镜像，你可以从镜像仓库拉取（例如Docker Hub），或者自行创建。[Docker Hub](https://hub.docker.com/explore/)上有成千上万的镜像.你当然也可以直接用命令`docker search`搜索所需的镜像。

这里需要重点解释一下 base 和 child 镜像之间的区别：

- **Base images** 基础镜像，该镜像没有父镜像，通常是一些基本的操作系统镜像如 ubuntu, busybox 或是 debian。

- **Child images** 子镜像，是基于基础镜像创建并增加了一些功能的镜像。

这里还有官方镜像和用户镜像的区别，他们都可以是基础镜像和子镜像。

- **官方镜像** 是官方支持的一些镜像，镜像名通常只有一个单词，在上面我们提及的镜像当中如 `python`, `ubuntu`, `busybox` 和 `hello-world` 都是官方镜像

- **用户镜像** 是你我这样的普通用户创建的镜像，往往是在基础镜像上添加了一些功能并提交的，这些镜像名的格式一般都是 `user/image-name`。

### 我们的第一个镜像

现在我们已经对镜像有一些更加深入的理解了，是时候创建一个你自己的镜像啦。这小节的目标就是创建一个简单的 [flask](http://falsk.pocoo.org) 应用。为了演示这个小李子，我已经创建了一个简单的[flask app](https://github.com/prakhar1989/docker-curriculum/tree/master/flask-app) 这个web网站每次打开的时候，将会随机展示一个猫咪的图片， 请clone项目并进入对应的文件夹中 -

```bash
$ git clone https://github.com/prakhar1989/docker-curriculum
$ cd docker-curriculum/flask-app
```

> 这个命令不应该在docker容器当中运行，而是在您的宿主操作系统的终端中运行

下一步则是如何创建一个属于这个web app的镜像，就像上面提及的，所有的用户镜像都是基于这个基础镜像的，由于我们的应用是用Python开发的，我们需要一个python的基础镜像，所以我们选择了一个[python3镜像](https://hub.docker.com/_/python/). 我们需要指定一个`TAG`，在这个例子当中，我们将会指定`python:3-onbuild` 这个版本作为我们的python镜像。

那么，什么是`onbuild` 版本呢?

> 这些镜像可能包括多个onbuild触发器，可能是在构建多种应用的时候需要的。 这个版本将会拷贝一个`requirements.txt`文件，然后运行`pip install`来安装依赖文件中定义的依赖依赖安装，然后拷贝当前的工作目录到`user/src/app`下。

换句话说，`onbuild`的版本表示这个镜像包括了一些需要支持这个应用运行的基本依赖工具，安装这些工具往往很麻烦。为了减少类似的工作，这个镜像已经帮你完成了大部分工作，因此可以直接使用，用来创建你自己的镜像，将其作为基础镜像。那么我们该如何创建我们自己的镜像呢？

### Dockerfile

[Dockerfile](https://docs.docker.com/engine/reference/builder/) 是一个简单的文本文件，包括一些用户创建镜像的命令，这些命令都由Docker客户端帮助运行。 这是一个非常简单的用户创建镜像的方法。
在编写Dockerfile中，我自己最喜欢的部分是，编写docker[命令](https://docs.docker.com/engine/reference/builder/#from) 几乎就像在编写通常的Linux命令一样，这也就意味着你不需要花费时间精力学习心得语法。

应用目录刚开始不包括Dockerfile,所以我们需要从头创建一个。为了开始我们新建一个空白的文本文件，然后将其保存到和我们的flask app相同的文件下。并且将其命名为`Docerfile`。

我们首先需要制定我们的基础镜像，用`FROM`关键字来指定 -

```dockerfile
FROM python:3-onbuild
```

下一步通常是写一些命令来拷贝一些文件以及安装文件，但是幸运的是，`onbuild`版本已经帮助我们完成了这个工作，所以我们不需要关注这一部分，下一步我们需要声明我们需要容器暴露的端口。由于我们的flask app通常运行在`5000`端口，因此我们需要指定它：

```dockerfile
EXPOSE 5000
```
最后一步我们需要输入命令运行我们的应用，其实就是简单的 `python ./app.py`。我们可以使用简单的[CMD](https://docs.docker.com/engine/reference/builder/#cmd) 完成这件事 -

```dockerfile
CMD ["python", "./app.py"]
```

`CMD`关键字的主要目的是指定容器启动的时候运行的命令。一旦指定了`CMD`我们的`Dockerfile`基本上已经完成了，现在它看起来像是这样的：

```dockerfile
# our base image
FROM python:3-onbuild

# specify the port number the container should expose
EXPOSE 5000

# run the application
CMD ["python", "./app.py"]
```

现在我们已经有了`Dockerfile`，我们可以创建我们的镜像了，通过`docker build`命令，Docker客户端将会帮助我们完成一大堆事情，它会读取`Dockerfile`并构建镜像。

接下来我将展示运行`docker build`之后的输出内容。在运行你自己的命令的时候，请一定把目标镜像的镜像名`/`之前的用户名换成你自己的，这个用户名应当和你在[Docker Hub]()上创建的一样。如果你没有注册账户，那么请先注册一个。`docker build`命令通常非常简单——需要提供`-t`选项，并提供一个镜像名称，最后需要指定一个有Dockerfile的文件夹路径。

```bash
$ docker build -t prakhar1989/catnip .
Sending build context to Docker daemon 8.704 kB
Step 1 : FROM python:3-onbuild
# Executing 3 build triggers...
Step 1 : COPY requirements.txt /usr/src/app/
 ---> Using cache
Step 1 : RUN pip install --no-cache-dir -r requirements.txt
 ---> Using cache
Step 1 : COPY . /usr/src/app
 ---> 1d61f639ef9e
Removing intermediate container 4de6ddf5528c
Step 2 : EXPOSE 5000
 ---> Running in 12cfcf6d67ee
 ---> f423c2f179d1
Removing intermediate container 12cfcf6d67ee
Step 3 : CMD python ./app.py
 ---> Running in f01401a5ace9
 ---> 13e87ed1fbc2
Removing intermediate container f01401a5ace9
Successfully built 13e87ed1fbc2
```

如果你没有`python:3-onbuild`镜像，客户端将会帮你先拉取这个镜像，并创建你自己的镜像，你的输出可能和我的会略有不同。请你注意你的镜像的on-build触发脚本是否正常执行，如果一切顺利，你可以运行`docker images`看看你的镜像是否创建成功。

最后一步就是运行你刚刚创建的镜像，看看是否正常创建（可能需要替换掉我的用户名）。

```bash
$ docker run -p 8888:5000 prakhar1989/catnip
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

这个命令将运行应用，并在容器内监听5000端口，但是由于我们在运行容器的时候，将容器内部的5000端口映射到了宿主系统的8888端口，因此应用实际上运行在了8888端口。

<picture>
  <source type="image/webp" srcset="images/catgif.webp">
  <img src="images/catgif.png" alt="cat gif website">
</picture>

恭喜你！你已经创建了你自己的第一个Docker镜像！

### Docker on AWS

What good is an application that can't be shared with friends, right? So in this section we are going to see how we can deploy our awesome application to the cloud so that we can share it with our friends! We're going to use AWS [Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/) to get our application up and running in a few clicks. We'll also see how easy it is to make our application scalable and manageable with Beanstalk!

#### Docker push

The first thing that we need to do before we deploy our app to AWS is to publish our image on a registry which can be accessed by AWS. There are many different [Docker registries](https://aws.amazon.com/ecr/) you can use (you can even host [your own](https://docs.docker.com/registry/deploying/)). For now, let's use [Docker Hub](https://hub.docker.com) to publish the image. To publish, just type

```bash
$ docker push prakhar1989/catnip
```

If this is the first time you are pushing an image, the client will ask you to login. Provide the same credentials that you used for logging into Docker Hub.

```bash
$ docker login
Username: prakhar1989
WARNING: login credentials saved in /Users/prakhar/.docker/config.json
Login Succeeded
```

Remember to replace the name of the image tag above with yours. It is important to have the format of `username/image_name` so that the client knows where to publish.

Once that is done, you can view your image on Docker Hub. For example, here's the [web page](https://hub.docker.com/r/prakhar1989/catnip/) for my image.

> Note: One thing that I'd like to clarify before we go ahead is that it is not **imperative** to host your image on a public registry (or any registry) in order to deploy to AWS. In case you're writing code for the next million-dollar unicorn startup you can totally skip this step. The reason why we're pushing our images publicly is that it makes deployment super simple by skipping a few intermediate configuration steps.

Now that your image is online, anyone who has docker installed can play with your app by typing just a single command.

```bash
$ docker run -p 8888:5000 prakhar1989/catnip
```

If you've pulled your hair in setting up local dev environments / sharing application configuration in the past, you very well know how awesome this sounds. That's why Docker is so cool!

#### Beanstalk

AWS Elastic Beanstalk (EB) is a PaaS (Platform as a Service) offered by AWS. If you've used Heroku, Google App Engine etc. you'll feel right at home. As a developer, you just tell EB how to run your app and it takes care of the rest - including scaling, monitoring and even updates. In April 2014, EB added support for running single-container Docker deployments which is what we'll use to deploy our app. Although EB has a very intuitive [CLI](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3.html), it does require some setup, and to keep things simple we'll use the web UI to launch our application.

To follow along, you need a functioning [AWS](http://aws.amazon.com) account. If you haven't already, please go ahead and do that now - you will need to enter your credit card information. But don't worry, it's free and anything we do in this tutorial will also be free! Let's get started.

Here are the steps:

- Login to your AWS [console](http://console.aws.amazon.com).
- Click on Elastic Beanstalk. It will be in the compute section on the top left. Alternatively, you can access the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk).

<picture>
  <source type="image/webp" srcset="images/eb-start.webp">
  <img src="images/eb-start.png" alt="Elastic Beanstalk start">
</picture>

- Click on "Create New Application" in the top right
- Give your app a memorable (but unique) name and provide an (optional) description
- In the **New Environment** screen, create a new environment and choose the **Web Server Environment**.
- Fill in the environment information by choosing a domain. This URL is what you'll share with your friends so make sure it's easy to remember.
- Under base configuration section. Choose *Docker* from the *predefined platform*.

<picture>
  <source type="image/webp" srcset="images/eb-docker.webp">
  <img src="images/eb-docker.jpeg" alt="Elastic Beanstalk Environment Type">
</picture>

- Now we need upload our application code. But since our application is packaged in a Docker container, we just to tell EB about our contianer. Open the `Dockerrun.aws.json` [file](https://github.com/prakhar1989/docker-curriculum/blob/master/flask-app/Dockerrun.aws.json) located in the `flask-app` folder and edit the `Name` of the image to your image's name. Don't worry, I'll explain the contents of the file shortly. When you are done, click on the radio button for "upload your own" and choose this file.
- The final screen that you see will have a few spinners indicating that your environment is being set up. It typically takes around 5 minutes for the first-time setup.

While we wait, let's quickly see what the `Dockerrun.aws.json` file contains. This file is basically an AWS specific file that tells EB details about our application and docker configuration.

```json
{
  "AWSEBDockerrunVersion": "1",
  "Image": {
    "Name": "prakhar1989/catnip",
    "Update": "true"
  },
  "Ports": [
    {
      "ContainerPort": "5000"
    }
  ],
  "Logging": "/var/log/nginx"
}
```

The file should be pretty self-explanatory, but you can always [reference](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker_image.html#create_deploy_docker_image_dockerrun) the official documentation for more information. We provide the name of the image that EB should use along with a port that the container should open.

Hopefully by now, our instance should be ready. Head over to the EB page and you should a green tick indicating that your app is alive and kicking.

<picture>
  <source type="image/webp" srcset="images/eb-deploy.webp">
  <img src="images/eb-deploy.png" alt="EB deploy">
</picture>

Go ahead and open the URL in your browser and you should see the application in all its glory. Feel free to email / IM / snapchat this link to your friends and family so that they can enjoy a few cat gifs, too.

Congratulations! You have deployed your first Docker application! That might seem like a lot of steps, but with the [command-line tool for EB](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3.html) you can almost mimic the functionality of Heroku in a few keystrokes! Hopefully you agree that Docker takes away a lot of the pains of building and deploying applications in the cloud. I would encourage you to read the AWS [documentation](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/docker-singlecontainer-deploy.html) on single-container Docker environments to get an idea of what features exist.

In the next (and final) part of the tutorial, we'll up the ante a bit and deploy an application that mimics the real-world more closely; an app with a persistent back-end storage tier. Let's get straight to it!

-------

## Multi-container Environments

In the last section, we saw how easy and fun it is to run applications with Docker. We started with a simple static website and then tried a Flask app. Both of which we could run locally and in the cloud with just a few commands. One thing both these apps had in common was that they were running in a **single container**.

Those of you who have experience running services in production know that usually apps nowadays are not that simple. There's almost always a database (or any other kind of persistent storage) involved. Systems such as [Redis](http://redis.io/) and [Memcached](http://memcached.org/) have become *de riguer* of most web application architectures. Hence, in this section we are going to spend some time learning how to Dockerize applications which rely on different services to run.

In particular, we are going to see how we can run and manage **multi-container** docker environments. Why multi-container you might ask? Well, one of the key points of Docker is the way it provides isolation. The idea of bundling a process with its dependencies in a sandbox (called containers) is what makes this so powerful.

Just like it's a good strategy to decouple your application tiers, it is wise to keep containers for each of the **services** separate. Each tier is likely to have different resource needs and those needs might grow at different rates. By separating the tiers into different containers, we can compose each tier using the most appropriate instance type based on different resource needs. This also plays in very well with the whole [microservices](http://martinfowler.com/articles/microservices.html) movement which is one of the main reasons why Docker (or any other container technology) is at the [forefront](https://medium.com/aws-activate-startup-blog/using-containers-to-build-a-microservices-architecture-6e1b8bacb7d1#.xl3wryr5z) of modern microservices architectures.

### SF Food Trucks

The app that we're going to Dockerize is called SF Food Trucks. My goal in building this app was to have something that is useful (in that it resembles a real-world application), relies on at least one service, but is not too complex for the purpose of this tutorial. This is what I came up with.

<picture>
  <source type="image/webp" srcset="images/foodtrucks.webp">
  <img src="images/foodtrucks.png" alt="SF Food Trucks">
</picture>

The app's backend is written in Python (Flask) and for search it uses [Elasticsearch](https://www.elastic.co/products/elasticsearch). Like everything else in this tutorial, the entire source is available on [Github](http://github.com/prakhar1989/FoodTrucks). We'll use this as our candidate application for learning out how to build, run and deploy a multi-container environment.

First up, lets clone the repository locally.

```bash
$ git clone https://github.com/prakhar1989/FoodTrucks
$ cd FoodTrucks
$ tree -L 2
.
├── Dockerfile
├── README.md
├── aws-compose.yml
├── docker-compose.yml
├── flask-app
│   ├── app.py
│   ├── package-lock.json
│   ├── package.json
│   ├── requirements.txt
│   ├── static
│   ├── templates
│   └── webpack.config.js
├── setup-aws-ecs.sh
├── setup-docker.sh
├── shot.png
└── utils
    ├── generate_geojson.py
    └── trucks.geojson
```

The `flask-app` folder contains the Python application, while the `utils` folder has some utilities to load the data into Elasticsearch. The directory also contains some YAML files and a Dockerfile, all of which we'll see in greater detail as we progress through this tutorial. If you are curious, feel free to take a look at the files.

Now that you're excited (hopefully), let's think of how we can Dockerize the app. We can see that the application consists of a Flask backend server and an Elasticsearch service. A natural way to split this app would be to have two containers - one running the Flask process and another running the Elasticsearch (ES) process. That way if our app becomes popular, we can scale it by adding more containers depending on where the bottleneck lies.

Great, so we need two containers. That shouldn't be hard right? We've already built our own Flask container in the previous section. And for Elasticsearch, let's see if we can find something on the hub.

```bash
$ docker search elasticsearch
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
elasticsearch                     Elasticsearch is a powerful open source se...   697       [OK]
itzg/elasticsearch                Provides an easily configurable Elasticsea...   17                   [OK]
tutum/elasticsearch               Elasticsearch image - listens in port 9200.     15                   [OK]
barnybug/elasticsearch            Latest Elasticsearch 1.7.2 and previous re...   15                   [OK]
digitalwonderland/elasticsearch   Latest Elasticsearch with Marvel & Kibana       12                   [OK]
monsantoco/elasticsearch          ElasticSearch Docker image                      9                    [OK]
```

Quite unsurprisingly, there exists an officially supported [image](https://store.docker.com/images/elasticsearch) for Elasticsearch. To get ES running, we can simply use `docker run` and have a single-node ES container running locally within no time.

> Note: Elastic, the company behind Elasticsearch, maintains its [own registry](https://www.docker.elastic.co/) for Elastic products. It's recommended to use the images from that registry if you plan to use Elasticsearch.

Let's first pull the image

```bash
$ docker pull docker.elastic.co/elasticsearch/elasticsearch:6.3.2
```

and then run it in development mode by specifying ports and setting an environment variable that configures Elasticsearch cluster to run as a single-node.

```bash
$ docker run -d --name es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:6.3.2
277451c15ec183dd939e80298ea4bcf55050328a39b04124b387d668e3ed3943
```

As seen above, we use `--name es` to give our container a name which makes it easy to use in subsequent commands. Once the container is started, we can see the logs by running `docker container logs` with the container name (or ID) to inspect the logs. You should logs similar to below if Elasticsearch started successfully.

> Note: Elasticsearch takes a few seconds to start so you might need to wait before you see `initialized` in the logs.

```bash
$ docker container ls
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED             STATUS              PORTS                                            NAMES
277451c15ec1        docker.elastic.co/elasticsearch/elasticsearch:6.3.2   "/usr/local/bin/dock…"   2 minutes ago       Up 2 minutes        0.0.0.0:9200->9200/tcp, 0.0.0.0:9300->9300/tcp   es

$ docker container logs es
[2018-07-29T05:49:09,304][INFO ][o.e.n.Node               ] [] initializing ...
[2018-07-29T05:49:09,385][INFO ][o.e.e.NodeEnvironment    ] [L1VMyzt] using [1] data paths, mounts [[/ (overlay)]], net usable_space [54.1gb], net total_space [62.7gb], types [overlay]
[2018-07-29T05:49:09,385][INFO ][o.e.e.NodeEnvironment    ] [L1VMyzt] heap size [990.7mb], compressed ordinary object pointers [true]
[2018-07-29T05:49:11,979][INFO ][o.e.p.PluginsService     ] [L1VMyzt] loaded module [x-pack-security]
[2018-07-29T05:49:11,980][INFO ][o.e.p.PluginsService     ] [L1VMyzt] loaded module [x-pack-sql]
[2018-07-29T05:49:11,980][INFO ][o.e.p.PluginsService     ] [L1VMyzt] loaded module [x-pack-upgrade]
[2018-07-29T05:49:11,980][INFO ][o.e.p.PluginsService     ] [L1VMyzt] loaded module [x-pack-watcher]
[2018-07-29T05:49:11,981][INFO ][o.e.p.PluginsService     ] [L1VMyzt] loaded plugin [ingest-geoip]
[2018-07-29T05:49:11,981][INFO ][o.e.p.PluginsService     ] [L1VMyzt] loaded plugin [ingest-user-agent]
[2018-07-29T05:49:17,659][INFO ][o.e.d.DiscoveryModule    ] [L1VMyzt] using discovery type [single-node]
[2018-07-29T05:49:18,962][INFO ][o.e.n.Node               ] [L1VMyzt] initialized
[2018-07-29T05:49:18,963][INFO ][o.e.n.Node               ] [L1VMyzt] starting ...
[2018-07-29T05:49:19,218][INFO ][o.e.t.TransportService   ] [L1VMyzt] publish_address {172.17.0.2:9300}, bound_addresses {0.0.0.0:9300}
[2018-07-29T05:49:19,302][INFO ][o.e.x.s.t.n.SecurityNetty4HttpServerTransport] [L1VMyzt] publish_address {172.17.0.2:9200}, bound_addresses {0.0.0.0:9200}
[2018-07-29T05:49:19,303][INFO ][o.e.n.Node               ] [L1VMyzt] started
[2018-07-29T05:49:19,439][WARN ][o.e.x.s.a.s.m.NativeRoleMappingStore] [L1VMyzt] Failed to clear cache for realms [[]]
[2018-07-29T05:49:19,542][INFO ][o.e.g.GatewayService     ] [L1VMyzt] recovered [0] indices into cluster_state
```

Now, lets try to see if can send a request to the Elasticsearch container. We use the `9200` port to send a `cURL` request to the container.

```bash
$ curl 0.0.0.0:9200
{
  "name" : "ijJDAOm",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "a_nSV3XmTCqpzYYzb-LhNw",
  "version" : {
    "number" : "6.3.2",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "053779d",
    "build_date" : "2018-07-20T05:20:23.451332Z",
    "build_snapshot" : false,
    "lucene_version" : "7.3.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

Sweet! It's looking good! While we are at it, let's get our Flask container running too. But before we get to that, we need a `Dockerfile`. In the last section, we used `python:3-onbuild` image as our base image. This time, however, apart from installing Python dependencies via `pip`, we want our application to also generate our minified Javascript file for production. For this, we'll require Nodejs. Since we need a custom build step, we'll start from the `ubuntu` base image to build our `Dockerfile` from scratch.

> Note: if you find that an existing image doesn't cater to your needs, feel free to start from another base image and tweak it yourself. For most of the images on Docker Hub, you should be able to find the corresponding `Dockerfile` on Github. Reading through existing Dockerfiles is one of the best ways to learn how to roll your own.

Our [Dockerfile](https://github.com/prakhar1989/FoodTrucks/blob/master/Dockerfile) for the flask app looks like below -

```dockerfile
# start from base
FROM ubuntu:14.04
MAINTAINER Prakhar Srivastav <prakhar@prakhar.me>

# install system-wide deps for python and node
RUN apt-get -yqq update
RUN apt-get -yqq install python-pip python-dev curl gnupg
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash
RUN apt-get install -yq nodejs

# copy our application code
ADD flask-app /opt/flask-app
WORKDIR /opt/flask-app

# fetch app specific deps
RUN npm install
RUN npm run build
RUN pip install -r requirements.txt

# expose port
EXPOSE 5000

# start app
CMD [ "python", "./app.py" ]
```

Quite a few new things here so let's quickly go over this file. We start off with the [Ubuntu LTS](https://wiki.ubuntu.com/LTS) base image and use the package manager `apt-get` to install the dependencies namely - Python and Node. The `yqq` flag is used to suppress output and assumes "Yes" to all prompts.

We then use the `ADD` command to copy our application into a new volume in the container - `/opt/flask-app`. This is where our code will reside. We also set this as our working directory, so that the following commands will be run in the context of this location. Now that our system-wide dependencies are installed, we get around to install app-specific ones. First off we tackle Node by installing the packages from npm and running the build command as defined in our `package.json` [file](https://github.com/prakhar1989/FoodTrucks/blob/master/flask-app/package.json#L7-L9). We finish the file off by installing the Python packages, exposing the port and defining the `CMD` to run as we did in the last section.

Finally, we can go ahead, build the image and run the container (replace `prakhar1989` with your username below).

```bash
$ git clone https://github.com/prakhar1989/FoodTrucks && cd FoodTrucks
$ docker build -t prakhar1989/foodtrucks-web .
```

In the first run, this will take some time as the Docker client will download the ubuntu image, run all the commands and prepare your image. Re-running `docker build` after any subsequent changes you make to the application code will almost be instantaneous. Now let's try running our app.

```bash
$ docker run -P --rm prakhar1989/foodtrucks-web
Unable to connect to ES. Retying in 5 secs...
Unable to connect to ES. Retying in 5 secs...
Unable to connect to ES. Retying in 5 secs...
Out of retries. Bailing out...
```

Oops! Our flask app was unable to run since it was unable to connect to Elasticsearch. How do we tell one container about the other container and get them to talk to each other? The answer lies in the next section.

###  Docker Network

Before we talk about the features Docker provides especially to deal with such scenarios, let's see if we can figure out a way to get around the problem. Hopefully this should give you an appreciation for the specific feature that we are going to study.

Okay, so let's run `docker container ls` (which is same as `docker ps`) and see what we have.

```bash
$ docker container ls
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED             STATUS              PORTS                                            NAMES
277451c15ec1        docker.elastic.co/elasticsearch/elasticsearch:6.3.2   "/usr/local/bin/dock…"   17 minutes ago      Up 17 minutes       0.0.0.0:9200->9200/tcp, 0.0.0.0:9300->9300/tcp   es
```

So we have one ES container running on `0.0.0.0:9200` port which we can directly access. If we can tell our Flask app to connect to this URL, it should be able to connect and talk to ES, right? Let's dig into our [Python code](https://github.com/prakhar1989/FoodTrucks/blob/master/flask-app/app.py#L7) and see how the connection details are defined.

```python
es = Elasticsearch(host='es')
```

To make this work, we need to tell the Flask container that the ES container is running on `0.0.0.0` host (the port by default is `9200`) and that should make it work, right? Unfortunately that is not correct since the IP `0.0.0.0` is the IP to access ES container from the **host machine** i.e. from my Mac. Another container will not be able to access this on the same IP address. Okay if not that IP, then which IP address should the ES container be accessible by? I'm glad you asked this question.

Now is a good time to start our exploration of networking in Docker. When docker is installed, it creates three networks automatically.

```bash
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
c2c695315b3a        bridge              bridge              local
a875bec5d6fd        host                host                local
ead0e804a67b        none                null                local
```

The **bridge** network is the network in which containers are run by default. So that means that when I ran the ES container, it was running in this bridge network. To validate this, let's inspect the network

```bash
$ docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "c2c695315b3aaf8fc30530bb3c6b8f6692cedd5cc7579663f0550dfdd21c9a26",
        "Created": "2018-07-28T20:32:39.405687265Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "277451c15ec183dd939e80298ea4bcf55050328a39b04124b387d668e3ed3943": {
                "Name": "es",
                "EndpointID": "5c417a2fc6b13d8ec97b76bbd54aaf3ee2d48f328c3f7279ee335174fbb4d6bb",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```

You can see that our container `277451c15ec1` is listed under the `Containers` section in the output. What we also see is the IP address this container has been allotted - `172.17.0.2`. Is this the IP address that we're looking for? Let's find out by running our flask container and trying to access this IP.

```bash
$ docker run -it --rm prakhar1989/foodtrucks-web bash
root@35180ccc206a:/opt/flask-app# curl 172.17.0.2:9200
{
  "name" : "Jane Foster",
  "cluster_name" : "elasticsearch",
  "version" : {
    "number" : "2.1.1",
    "build_hash" : "40e2c53a6b6c2972b3d13846e450e66f4375bd71",
    "build_timestamp" : "2015-12-15T13:05:55Z",
    "build_snapshot" : false,
    "lucene_version" : "5.3.1"
  },
  "tagline" : "You Know, for Search"
}
root@35180ccc206a:/opt/flask-app# exit
```

This should be fairly straightforward to you by now. We start the container in the interactive mode with the `bash` process. The `--rm` is a convenient flag for running one off commands since the container gets cleaned up when it's work is done. We try a `curl` but we need to install it first. Once we do that, we see that we can indeed talk to ES on `172.17.0.2:9200`. Awesome!

Although we have figured out a way to make the containers talk to each other, there are still two problems with this approach -

1. How do we tell the Flask container that `es` hostname stands for `172.17.0.2` or some other IP since the IP can change?

2. Since the *bridge* network is shared by every container by default, this method is **not secure**. How do we isolate our network?

The good news that Docker has a great answer to our questions. It allows us to define our own networks while keeping them isolated using the `docker network` command.

Let's first go ahead and create our own network.

```bash
$ docker network create foodtrucks-net
0815b2a3bb7a6608e850d05553cc0bda98187c4528d94621438f31d97a6fea3c

$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
c2c695315b3a        bridge              bridge              local
0815b2a3bb7a        foodtrucks-net      bridge              local
a875bec5d6fd        host                host                local
ead0e804a67b        none                null                local
```

The `network create` command creates a new *bridge* network, which is what we need at the moment. In terms of Docker, a bridge network uses a software bridge which allows containers connected to the same bridge network to communicate, while providing isolation from containers which are not connected to that bridge network. The Docker bridge driver automatically installs rules in the host machine so that containers on different bridge networks cannot communicate directly with each other. There are other kinds of networks that you can create, and you are encouraged to read about them in the official [docs](https://docs.docker.com/engine/userguide/networking/dockernetworks/).

Now that we have a network, we can launch our containers inside this network using the `--net` flag. Let's do that - but first, we will stop our ES container that is running in the bridge (default) network.

```bash
$ docker container stop es
es

$ docker run -d --name es --net foodtrucks-net -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:6.3.2
13d6415f73c8d88bddb1f236f584b63dbaf2c3051f09863a3f1ba219edba3673

$ docker network inspect foodtrucks-net
[
    {
        "Name": "foodtrucks-net",
        "Id": "0815b2a3bb7a6608e850d05553cc0bda98187c4528d94621438f31d97a6fea3c",
        "Created": "2018-07-30T00:01:29.1500984Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "13d6415f73c8d88bddb1f236f584b63dbaf2c3051f09863a3f1ba219edba3673": {
                "Name": "es",
                "EndpointID": "29ba2d33f9713e57eb6b38db41d656e4ee2c53e4a2f7cf636bdca0ec59cd3aa7",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

As you can see, our `es` container is now running inside the `foodtrucks-net` bridge network. Now let's inspect what happens when we launch in our `foodtrucks-net` network.

```bash
$ docker run -it --rm --net foodtrucks-net prakhar1989/foodtrucks-web bash
root@9d2722cf282c:/opt/flask-app# curl es:9200
{
  "name" : "wWALl9M",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "BA36XuOiRPaghPNBLBHleQ",
  "version" : {
    "number" : "6.3.2",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "053779d",
    "build_date" : "2018-07-20T05:20:23.451332Z",
    "build_snapshot" : false,
    "lucene_version" : "7.3.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
root@53af252b771a:/opt/flask-app# ls
app.py  node_modules  package.json  requirements.txt  static  templates  webpack.config.js
root@53af252b771a:/opt/flask-app# python app.py
Index not found...
Loading data in elasticsearch ...
Total trucks loaded:  733
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
root@53af252b771a:/opt/flask-app# exit
```

Wohoo! That works! On user-defined networks like foodtrucks-net, containers can not only communicate by IP address, but can also resolve a container name to an IP address. This capability is called *automatic service discovery*. Great! Let's launch our Flask container for real now -

```bash
$ docker run -d --net foodtrucks-net -p 5000:5000 --name foodtrucks-web prakhar1989/foodtrucks-web
852fc74de2954bb72471b858dce64d764181dca0cf7693fed201d76da33df794

$ docker container ls
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED              STATUS              PORTS                                            NAMES
852fc74de295        prakhar1989/foodtrucks-web                            "python ./app.py"        About a minute ago   Up About a minute   0.0.0.0:5000->5000/tcp                           foodtrucks-web
13d6415f73c8        docker.elastic.co/elasticsearch/elasticsearch:6.3.2   "/usr/local/bin/dock…"   17 minutes ago       Up 17 minutes       0.0.0.0:9200->9200/tcp, 0.0.0.0:9300->9300/tcp   es

$ curl -I 0.0.0.0:5000
HTTP/1.0 200 OK
Content-Type: text/html; charset=utf-8
Content-Length: 3697
Server: Werkzeug/0.11.2 Python/2.7.6
Date: Sun, 10 Jan 2016 23:58:53 GMT
```

Head over to [http://0.0.0.0:5000](http://0.0.0.0:5000) and see your glorious app live! Although that might have seemed like a lot of work, we actually just typed 4 commands to go from zero to running. I've collated the commands in a [bash script](https://github.com/prakhar1989/FoodTrucks/blob/master/setup-docker.sh).

```bash
#!/bin/bash

# build the flask container
docker build -t prakhar1989/foodtrucks-web .

# create the network
docker network create foodtrucks-net

# start the ES container
docker run -d --name es --net foodtrucks-net -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:6.3.2

# start the flask app container
docker run -d --net foodtrucks-net -p 5000:5000 --name foodtrucks-web prakhar1989/foodtrucks-web
```

Now imagine you are distributing your app to a friend, or running on a server that has docker installed. You can get a whole app running with just one command!

```bash
$ git clone https://github.com/prakhar1989/FoodTrucks
$ cd FoodTrucks
$ ./setup-docker.sh
```

And that's it! If you ask me, I find this to be an extremely awesome, and a powerful way of sharing and running your applications!

### Docker Compose

Till now we've spent all our time exploring the Docker client. In the Docker ecosystem, however, there are a bunch of other open-source tools which play very nicely with Docker. A few of them are -

1. [Docker Machine](https://docs.docker.com/machine/) - Create Docker hosts on your computer, on cloud providers, and inside your own data center
2. [Docker Compose](https://docs.docker.com/compose/) - A tool for defining and running multi-container Docker applications.
3. [Docker Swarm](https://docs.docker.com/swarm/) - A native clustering solution for Docker
4. [Kubernetes](https://kubernetes.io) - Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.

In this section, we are going to look at one of these tools, Docker Compose, and see how it can make dealing with multi-container apps easier.

The background story of Docker Compose is quite interesting. Roughly two years ago, a company called OrchardUp launched a tool called Fig. The idea behind Fig was to make isolated development environments work with Docker. The project was very well received on [Hacker News](https://news.ycombinator.com/item?id=7132044) - I oddly remember reading about it but didn't quite get the hang of it.

The [first comment](https://news.ycombinator.com/item?id=7133449) on the forum actually does a good job of explaining what Fig is all about.

> So really at this point, that's what Docker is about: running processes. Now Docker offers a quite rich API to run the processes: shared volumes (directories) between containers (i.e. running images), forward port from the host to the container, display logs, and so on.  But that's it: Docker as of now, remains at the process level.

> While it provides options to orchestrate multiple containers to create a single "app", it doesn't address the managemement of such group of containers as a single entity.
> And that's where tools such as Fig come in: talking about a group of containers as a single entity. Think "run an app" (i.e. "run an orchestrated cluster of containers") instead of "run a container".

It turns out that a lot of people using docker agree with this sentiment. Slowly and steadily as Fig became popular, Docker Inc. took notice, acquired the company and re-branded Fig as Docker Compose.

So what is *Compose* used for? Compose is a tool that is used for defining and running multi-container Docker apps in an easy way. It provides a configuration file called `docker-compose.yml` that can be used to bring up an application and the suite of services it depends on with just one command. Compose works in all environments: production, staging, development, testing, as well as CI workflows, although Compose is ideal for development and testing environments.

Let's see if we can create a `docker-compose.yml` file for our SF-Foodtrucks app and evaluate whether Docker Compose lives up to its promise.

The first step, however, is to install Docker Compose. If you're running Windows or Mac, Docker Compose is already installed as it comes in the Docker Toolbox. Linux users can easily get their hands on Docker Compose by following the [instructions](https://docs.docker.com/compose/install/) on the docs. Since Compose is written in Python, you can also simply do `pip install docker-compose`. Test your installation with -

```bash
$ docker-compose --version
docker-compose version 1.21.2, build a133471
```

Now that we have it installed, we can jump on the next step i.e. the Docker Compose file `docker-compose.yml`. The syntax for YAML is quite simple and the repo already contains the docker-compose [file](https://github.com/prakhar1989/FoodTrucks/blob/master/docker-compose.yml) that we'll be using.

```yaml
version: "3"
services:
  es:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.3.2
    container_name: es
    environment:
      - discovery.type=single-node
    ports:
      - 9200:9200
    volumes:
      - esdata1:/usr/share/elasticsearch/data
  web:
    image: prakhar1989/foodtrucks-web
    command: python app.py
    depends_on:
      - es
    ports:
      - 5000:5000
    volumes:
      - ./flask-app:/opt/flask-app
volumes:
    esdata1:
      driver: local
```

Let me breakdown what the file above means. At the parent level, we define the names of our services - `es` and `web`. For each service, that Docker needs to run, we can add additional parameters out of which `image` is required. For `es`, we just refer to the `elasticsearch` image available on Elastic registry. For our Flask app, we refer to the image that we built at the beginning of this section.

Via other parameters such as `command` and `ports` we provide more information about the container. The `volumes` parameter specifies a mount point in our `web` container where the code will reside. This is purely optional and is useful if you need access to logs etc. We'll later see how this can be useful during development. Refer to the [online reference](https://docs.docker.com/compose/compose-file) to learn more about the parameters this file supports. We also add volumes for `es` container so that the data we load persists between restarts. We also specify `depends_on`, which tells docker to start the `es` container before `web`. You can read more about it on [docker compose docs](https://docs.docker.com/compose/compose-file/#depends_on).

> Note: You must be inside the directory with the `docker-compose.yml` file in order to execute most Compose commands.

Great! Now the file is ready, let's see `docker-compose` in action. But before we start, we need to make sure the ports are free. So if you have the Flask and ES containers running, lets turn them off.

```bash
$ docker stop $(docker ps -q)
39a2f5df14ef
2a1b77e066e6
```

Now we can run `docker-compose`. Navigate to the food trucks directory and run `docker-compose up`.

```bash
$ docker-compose up
Creating network "foodtrucks_default" with the default driver
Creating foodtrucks_es_1
Creating foodtrucks_web_1
Attaching to foodtrucks_es_1, foodtrucks_web_1
es_1  | [2016-01-11 03:43:50,300][INFO ][node                     ] [Comet] version[2.1.1], pid[1], build[40e2c53/2015-12-15T13:05:55Z]
es_1  | [2016-01-11 03:43:50,307][INFO ][node                     ] [Comet] initializing ...
es_1  | [2016-01-11 03:43:50,366][INFO ][plugins                  ] [Comet] loaded [], sites []
es_1  | [2016-01-11 03:43:50,421][INFO ][env                      ] [Comet] using [1] data paths, mounts [[/usr/share/elasticsearch/data (/dev/sda1)]], net usable_space [16gb], net total_space [18.1gb], spins? [possibly], types [ext4]
es_1  | [2016-01-11 03:43:52,626][INFO ][node                     ] [Comet] initialized
es_1  | [2016-01-11 03:43:52,632][INFO ][node                     ] [Comet] starting ...
es_1  | [2016-01-11 03:43:52,703][WARN ][common.network           ] [Comet] publish address: {0.0.0.0} is a wildcard address, falling back to first non-loopback: {172.17.0.2}
es_1  | [2016-01-11 03:43:52,704][INFO ][transport                ] [Comet] publish_address {172.17.0.2:9300}, bound_addresses {[::]:9300}
es_1  | [2016-01-11 03:43:52,721][INFO ][discovery                ] [Comet] elasticsearch/cEk4s7pdQ-evRc9MqS2wqw
es_1  | [2016-01-11 03:43:55,785][INFO ][cluster.service          ] [Comet] new_master {Comet}{cEk4s7pdQ-evRc9MqS2wqw}{172.17.0.2}{172.17.0.2:9300}, reason: zen-disco-join(elected_as_master, [0] joins received)
es_1  | [2016-01-11 03:43:55,818][WARN ][common.network           ] [Comet] publish address: {0.0.0.0} is a wildcard address, falling back to first non-loopback: {172.17.0.2}
es_1  | [2016-01-11 03:43:55,819][INFO ][http                     ] [Comet] publish_address {172.17.0.2:9200}, bound_addresses {[::]:9200}
es_1  | [2016-01-11 03:43:55,819][INFO ][node                     ] [Comet] started
es_1  | [2016-01-11 03:43:55,826][INFO ][gateway                  ] [Comet] recovered [0] indices into cluster_state
es_1  | [2016-01-11 03:44:01,825][INFO ][cluster.metadata         ] [Comet] [sfdata] creating index, cause [auto(index api)], templates [], shards [5]/[1], mappings [truck]
es_1  | [2016-01-11 03:44:02,373][INFO ][cluster.metadata         ] [Comet] [sfdata] update_mapping [truck]
es_1  | [2016-01-11 03:44:02,510][INFO ][cluster.metadata         ] [Comet] [sfdata] update_mapping [truck]
es_1  | [2016-01-11 03:44:02,593][INFO ][cluster.metadata         ] [Comet] [sfdata] update_mapping [truck]
es_1  | [2016-01-11 03:44:02,708][INFO ][cluster.metadata         ] [Comet] [sfdata] update_mapping [truck]
es_1  | [2016-01-11 03:44:03,047][INFO ][cluster.metadata         ] [Comet] [sfdata] update_mapping [truck]
web_1 |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

Head over to the IP to see your app live. That was amazing wasn't it? Just few lines of configuration and we have two Docker containers running successfully in unison. Let's stop the services and re-run in detached mode.

```bash
web_1 |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
Killing foodtrucks_web_1 ... done
Killing foodtrucks_es_1 ... done

$ docker-compose up -d
Creating es               ... done
Creating foodtrucks_web_1 ... done

$ docker-compose ps
      Name                    Command               State                Ports
--------------------------------------------------------------------------------------------
es                 /usr/local/bin/docker-entr ...   Up      0.0.0.0:9200->9200/tcp, 9300/tcp
foodtrucks_web_1   python app.py                    Up      0.0.0.0:5000->5000/tcp
```

Unsurprisingly, we can see both the containers running successfully. Where do the names come from? Those were created automatically by Compose. But does *Compose* also create the network automatically? Good question! Let's find out.

First off, let us stop the services from running. We can always bring them back up in just one command. Data volumes will persist, so it’s possible to start the cluster again with the same data using docker-compose up. To destroy the cluster and the data volumes, just type `docker-compose down -v`.

```bash
$ docker-compose down -v
Stopping foodtrucks_web_1 ... done
Stopping es               ... done
Removing foodtrucks_web_1 ... done
Removing es               ... done
Removing network foodtrucks_default
Removing volume foodtrucks_esdata1
```

While we're are at it, we'll also remove the `foodtrucks` network that we created last time.

```bash
$ docker network rm foodtrucks-net
$ docker network ls
NETWORK ID          NAME                 DRIVER              SCOPE
c2c695315b3a        bridge               bridge              local
a875bec5d6fd        host                 host                local
ead0e804a67b        none                 null                local
```

Great! Now that we have a clean slate, let's re-run our services and see if *Compose* does it's magic.

```bash
$ docker-compose up -d
Recreating foodtrucks_es_1
Recreating foodtrucks_web_1

$ docker container ls
CONTAINER ID        IMAGE                        COMMAND                  CREATED             STATUS              PORTS                    NAMES
f50bb33a3242        prakhar1989/foodtrucks-web   "python app.py"          14 seconds ago      Up 13 seconds       0.0.0.0:5000->5000/tcp   foodtrucks_web_1
e299ceeb4caa        elasticsearch                "/docker-entrypoint.s"   14 seconds ago      Up 14 seconds       9200/tcp, 9300/tcp       foodtrucks_es_1
```

So far, so good. Time to see if any networks were created.

```bash
$ docker network ls
NETWORK ID          NAME                 DRIVER
c2c695315b3a        bridge               bridge              local
f3b80f381ed3        foodtrucks_default   bridge              local
a875bec5d6fd        host                 host                local
ead0e804a67b        none                 null                local
```

You can see that compose went ahead and created a new network called `foodtrucks_default` and attached both the new services in that network so that each of these are discoverable to the other. Each container for a service joins the default network and is both reachable by other containers on that network, and discoverable by them at a hostname identical to the container name. 

```bash
$ docker ps
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED              STATUS              PORTS                              NAMES
8c6bb7e818ec        docker.elastic.co/elasticsearch/elasticsearch:6.3.2   "/usr/local/bin/dock…"   About a minute ago   Up About a minute   0.0.0.0:9200->9200/tcp, 9300/tcp   es
7640cec7feb7        prakhar1989/foodtrucks-web                            "python app.py"          About a minute ago   Up About a minute   0.0.0.0:5000->5000/tcp             foodtrucks_web_1

$ docker network inspect foodtrucks_default
[
    {
        "Name": "foodtrucks_default",
        "Id": "f3b80f381ed3e03b3d5e605e42c4a576e32d38ba24399e963d7dad848b3b4fe7",
        "Created": "2018-07-30T03:36:06.0384826Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": true,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "7640cec7feb7f5615eaac376271a93fb8bab2ce54c7257256bf16716e05c65a5": {
                "Name": "foodtrucks_web_1",
                "EndpointID": "b1aa3e735402abafea3edfbba605eb4617f81d94f1b5f8fcc566a874660a0266",
                "MacAddress": "02:42:ac:13:00:02",
                "IPv4Address": "172.19.0.2/16",
                "IPv6Address": ""
            },
            "8c6bb7e818ec1f88c37f375c18f00beb030b31f4b10aee5a0952aad753314b57": {
                "Name": "es",
                "EndpointID": "649b3567d38e5e6f03fa6c004a4302508c14a5f2ac086ee6dcf13ddef936de7b",
                "MacAddress": "02:42:ac:13:00:03",
                "IPv4Address": "172.19.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {
            "com.docker.compose.network": "default",
            "com.docker.compose.project": "foodtrucks",
            "com.docker.compose.version": "1.21.2"
        }
    }
]
```

### Development Workflow

Before we jump to the next section, there's one last thing I wanted to cover about docker-compose. As stated earlier, docker-compose is really great for development and testing. So let's see how we can configure compose to make our lives easier during development.

Throughout this tutorial, we've worked with readymade docker images. While we've built images from scratch, we haven't touched any application code yet and mostly restricted ourselves to editing Dockerfiles and YAML configurations. One thing that you must be wondering is how does the workflow look during development? Is one supposed to keep creating Docker images for every change, then publish it and then run it to see if the changes works as expected? I'm sure that sounds super tedious. There has to be a better way. In this section, that's what we're going to explore.

Let's see how we can make a change in the Foodtrucks app we just ran. Make sure you have the app running,

```bash
$ docker container ls
CONTAINER ID        IMAGE                                                 COMMAND                  CREATED             STATUS              PORTS                              NAMES
5450ebedd03c        prakhar1989/foodtrucks-web                            "python app.py"          9 seconds ago       Up 6 seconds        0.0.0.0:5000->5000/tcp             foodtrucks_web_1
05d408b25dfe        docker.elastic.co/elasticsearch/elasticsearch:6.3.2   "/usr/local/bin/dock…"   10 hours ago        Up 10 hours         0.0.0.0:9200->9200/tcp, 9300/tcp   es
```

Now let's see if we can change this app to display a `Hello world!` message when a request is made to `/hello` route. Currently, the app responds with a 404.

```bash
$ curl -I 0.0.0.0:5000/hello
HTTP/1.0 404 NOT FOUND
Content-Type: text/html
Content-Length: 233
Server: Werkzeug/0.11.2 Python/2.7.15rc1
Date: Mon, 30 Jul 2018 15:34:38 GMT
```

Why does this happen? Since ours is a Flask app, we can see [`app.py`]() for answers. In Flask, routes are defined with @app.route syntax. In the file, you'll see that we only have three routes defined - `/`, `/debug` and `/search`. The `/` route renders the main app, the `debug` route is used to return some debug information and finally `search` is used by the app to query elasticsearch.

```bash
$ curl 0.0.0.0:5000/debug
{
  "msg": "yellow open sfdata Ibkx7WYjSt-g8NZXOEtTMg 5 1 618 0 1.3mb 1.3mb\n",
  "status": "success"
}
```

Given that context, how would we add a new route for `hello`? You guessed it! Let's open `flask-app/app.py` in our favorite editor and make the following change

```python
@app.route('/')
def index():
  return render_template("index.html")

# add a new hello route
@app.route('/hello')
def hello():
  return "hello world!"
```

Now let's try making a request again

```bash
$ curl -I 0.0.0.0:5000/hello
HTTP/1.0 404 NOT FOUND
Content-Type: text/html
Content-Length: 233
Server: Werkzeug/0.11.2 Python/2.7.15rc1
Date: Mon, 30 Jul 2018 15:34:38 GMT
```

Oh no! That didn't work! What did we do wrong? While we did make the change in `app.py`, the file resides in our machine (or the host machine), but since Docker is running our containers based off the `prakhar1989/foodtrucks-web` image, it doesn't know about this change. To validate this, lets try the following - 

```
$ docker-compose run web bash
Starting es ... done
root@581e351c82b0:/opt/flask-app# ls
app.py        package-lock.json  requirements.txt  templates
node_modules  package.json       static            webpack.config.js
root@581e351c82b0:/opt/flask-app# grep hello app.py
root@581e351c82b0:/opt/flask-app# exit
```

What we're trying to do here is to validate that our changes are not in the `app.py` that's running in the container. We do this by running the command `docker compose run`, which is similar to its cousin `docker run` but takes additional arguments for the service (which is `web` in our [case](https://github.com/prakhar1989/FoodTrucks/blob/master/docker-compose.yml#L12)). As soon as we run `bash`, the shell opens in `/opt/flask-app` as specified in our [Dockerfile](https://github.com/prakhar1989/FoodTrucks/blob/master/Dockerfile#L13). From the grep command we can see that our changes are not in the file.

Lets see how we can fix it. First off, we need to tell docker compose to not use the image and instead use the files locally. We'll also set debug mode to `true` so that Flask knows to reload the server when `app.py` changes. Replace the `web` portion of the `docker-compose.yml` file like so:

```yaml
version: "3"
services:
  es:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.3.2
    container_name: es
    environment:
      - discovery.type=single-node
    ports:
      - 9200:9200
    volumes:
      - esdata1:/usr/share/elasticsearch/data
  web:
    build: . # replaced image with build
    command: python app.py
    environment:
      - DEBUG=True  # set an env var for flask
    depends_on:
      - es
    ports:
      - "5000:5000"
    volumes:
      - ./flask-app:/opt/flask-app
volumes:
    esdata1:
      driver: local
```

With that change ([diff](https://github.com/prakhar1989/FoodTrucks/commit/31368936de5959efaf4457a94c678d21e3eefbce)), let's stop and start the containers.

```bash
$ docker-compose down -v
Stopping foodtrucks_web_1 ... done
Stopping es               ... done
Removing foodtrucks_web_1 ... done
Removing es               ... done
Removing network foodtrucks_default
Removing volume foodtrucks_esdata1

$ docker-compose up -d
Creating network "foodtrucks_default" with the default driver
Creating volume "foodtrucks_esdata1" with local driver
Creating es ... done
Creating foodtrucks_web_1 ... done
```

As a final step, lets make the change in `app.py` by adding a new route. Now we try to curl

```bash
$ curl 0.0.0.0:5000/hello
hello world
```

Wohoo! We get a valid response! Try playing around by making more changes in the app. 

That concludes our tour of Docker Compose. With Docker Compose, you can also pause your services, run a one-off command on a container and even scale the number of containers. I also recommend you checkout a few other [use-cases](https://docs.docker.com/compose/overview/#common-use-cases) of Docker compose. Hopefully I was able to show you how easy it is to manage multi-container environments with Compose. In the final section, we are going to deploy our app to AWS!

### AWS Elastic Container Service

In the last section we used `docker-compose` to run our app locally with a single command: `docker-compose up`. Now that we have a functioning app we want to share this with the world, get some users, make tons of money and buy a big house in Miami. Executing the last three are beyond the scope of tutorial, so we'll spend our time instead on figuring out how we can deploy our multi-container apps on the cloud with AWS.

If you've read this far you are much pretty convinced that Docker is a pretty cool technology. And you are not alone. Seeing the meteoric rise of Docker, almost all Cloud vendors started working on adding support for deploying Docker apps on their platform. As of today, you can deploy containers on [Google Cloud Platform](https://cloud.google.com/containers/), [AWS](https://aws.amazon.com/containers/), [Azure](https://azure.microsoft.com/en-us/overview/containers/) and many others. We already got a primer on deploying single container apps with Elastic Beanstalk and in this section we are going to look at [Elastic Container Service (or ECS)](https://aws.amazon.com/ecs/) by AWS.

AWS ECS is a scalable and super flexible container management service that supports Docker containers. It allows you to operate a Docker cluster on top of EC2 instances via an easy-to-use API. Where Beanstalk came with reasonable defaults, ECS allows you to completely tune your environment as per your needs. This makes ECS, in my opinion, quite complex to get started with.

Luckily for us, ECS has a friendly [CLI](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI.html) tool that understands Docker Compose files and automatically provisions the cluster on ECS! Since we already have a functioning `docker-compose.yml` it should not take a lot of effort in getting up and running on AWS. So let's get started!

The first step is to install the CLI. Instructions to install the CLI on both Mac and Linux are explained very clearly in the [official docs](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_installation.html). Go ahead, install the CLI and when you are done, verify the install by running

```bash
$ ecs-cli --version
ecs-cli version 0.1.0 (*cbdc2d5)
```

The first step is to get a keypair which we'll be using to log into the instances. Head over to your [EC2 Console](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#KeyPairs:sort=keyName) and create a new keypair. Download the keypair and store it in a safe location. Another thing to note before you move away from this screen is the region name. In my case, I have named my key - `ecs` and set my region as `us-east-1`. This is what I'll assume for the rest of this walkthrough.

<picture>
  <source type="image/webp" srcset="images/keypair.webp">
  <img src="images/keypair.png" alt="EC2 Keypair">
</picture>

The next step is to configure the CLI.

```bash
$ ecs-cli configure --region us-east-1 --cluster foodtrucks
INFO[0000] Saved ECS CLI configuration for cluster (foodtrucks)
```

We provide the `configure` command with the region name we want our cluster to reside in and a cluster name. Make sure you provide the **same region name** that you used when creating the keypair. If you've not configured the [AWS CLI](https://aws.amazon.com/cli/) on your computer before, you can use the official [guide](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html), which explains everything in great detail on how to get everything going.

The next step enables the CLI to create a [CloudFormation](https://aws.amazon.com/cloudformation/) template.

```bash
$ ecs-cli up --keypair ecs --capability-iam --size 2 --instance-type t2.micro
INFO[0000] Created cluster                               cluster=foodtrucks
INFO[0001] Waiting for your cluster resources to be created
INFO[0001] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
INFO[0061] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
INFO[0122] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
INFO[0182] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
INFO[0242] Cloudformation stack status                   stackStatus=CREATE_IN_PROGRESS
```

Here we provide the name of the keypair we downloaded initially (`ecs` in my case), the number of instances that we want to use (`--size`) and the type of instances that we want the containers to run on. The `--capability-iam` flag tells the CLI that we acknowledge that this command may create IAM resources.

The last and final step is where we'll use our `docker-compose.yml` file. We'll need to make a tiny change, so instead of modifying the original, let's make a copy of it and call it `aws-compose.yml`. The contents of [this file](https://github.com/prakhar1989/FoodTrucks/blob/master/aws-compose.yml) (after making the changes) look like (below) -

```bash
es:
  image: elasticsearch
  cpu_shares: 100
  mem_limit: 262144000
web:
  image: prakhar1989/foodtrucks-web
  cpu_shares: 100
  mem_limit: 262144000
  ports:
    - "80:5000"
  links:
    - es
```

The only changes we made from the original `docker-compose.yml` are of providing the `mem_limit` and `cpu_shares` values for each container. We also got rid of the `version` and the `services` key, since AWS doesn't yet support [version 2](https://docs.docker.com/compose/compose-file/#version-2) of Compose file format. Since our apps will run on `t2.micro` instances, we allocate 250mb of memory. Another thing we need to do before we move onto the next step is to publish our image on Docker Hub. As of this writing, ecs-cli **does not** support the `build` command - which is [supported](https://docs.docker.com/compose/compose-file/#build) perfectly by Docker Compose.

```bash
$ docker push prakhar1989/foodtrucks-web
```

Great! Now let's run the final command that will deploy our app on ECS!

```bash
$ ecs-cli compose --file aws-compose.yml up
INFO[0000] Using ECS task definition                     TaskDefinition=ecscompose-foodtrucks:2
INFO[0000] Starting container...                         container=845e2368-170d-44a7-bf9f-84c7fcd9ae29/es
INFO[0000] Starting container...                         container=845e2368-170d-44a7-bf9f-84c7fcd9ae29/web
INFO[0000] Describe ECS container status                 container=845e2368-170d-44a7-bf9f-84c7fcd9ae29/web desiredStatus=RUNNING lastStatus=PENDING taskDefinition=ecscompose-foodtrucks:2
INFO[0000] Describe ECS container status                 container=845e2368-170d-44a7-bf9f-84c7fcd9ae29/es desiredStatus=RUNNING lastStatus=PENDING taskDefinition=ecscompose-foodtrucks:2
INFO[0036] Describe ECS container status                 container=845e2368-170d-44a7-bf9f-84c7fcd9ae29/es desiredStatus=RUNNING lastStatus=PENDING taskDefinition=ecscompose-foodtrucks:2
INFO[0048] Describe ECS container status                 container=845e2368-170d-44a7-bf9f-84c7fcd9ae29/web desiredStatus=RUNNING lastStatus=PENDING taskDefinition=ecscompose-foodtrucks:2
INFO[0048] Describe ECS container status                 container=845e2368-170d-44a7-bf9f-84c7fcd9ae29/es desiredStatus=RUNNING lastStatus=PENDING taskDefinition=ecscompose-foodtrucks:2
INFO[0060] Started container...                          container=845e2368-170d-44a7-bf9f-84c7fcd9ae29/web desiredStatus=RUNNING lastStatus=RUNNING taskDefinition=ecscompose-foodtrucks:2
INFO[0060] Started container...                          container=845e2368-170d-44a7-bf9f-84c7fcd9ae29/es desiredStatus=RUNNING lastStatus=RUNNING taskDefinition=ecscompose-foodtrucks:2
```

It's not a coincidence that the invocation above looks similar to the one we used with **Docker Compose**. The `--file` argument is used to override the default file (`docker-compose.yml`) that the CLI will read. If everything went well, you should see a `desiredStatus=RUNNING lastStatus=RUNNING` as the last line.

Awesome! Our app is live, but how can we access it?

```bash
ecs-cli ps
Name                                      State    Ports                     TaskDefinition
845e2368-170d-44a7-bf9f-84c7fcd9ae29/web  RUNNING  54.86.14.14:80->5000/tcp  ecscompose-foodtrucks:2
845e2368-170d-44a7-bf9f-84c7fcd9ae29/es   RUNNING                            ecscompose-foodtrucks:2
```

Go ahead and open [http://54.86.14.14](http://54.86.14.14) in your browser and you should see the Food Trucks in all its black-yellow glory!
Since we're on the topic, let's see how our [AWS ECS](https://console.aws.amazon.com/ecs/home?region=us-east-1#/clusters) console looks.

<picture>
  <source type="image/webp" srcset="images/cluster.webp">
  <img src="images/cluster.png" alt="Cluster">
</picture>

<picture>
  <source type="image/webp" srcset="images/tasks.webp">
  <img src="images/tasks.png" alt="Tasks">
</picture>

We can see above that our ECS cluster called 'foodtrucks' was created and is now running 1 task with 2 container instances. Spend some time browsing this console to get a hang of all the options that are here.

So there you have it. With just a few commands we were able to deploy our awesome app on the AWS cloud!

___________

## Conclusion

And that's a wrap! After a long, exhaustive but fun tutorial you are now ready to take the container world by storm! If you followed along till the very end then you should definitely be proud of yourself. You learnt how to setup Docker, run your own containers, play with static and dynamic websites and most importantly got hands on experience with deploying your applications to the cloud!

I hope that finishing this tutorial makes you more confident in your abilities to deal with servers. When you have an idea of building your next app, you can be sure that you'll be able to get it in front of people with minimal effort.

### Next Steps

Your journey into the container world has just started! My goal with this tutorial was to whet your appetite and show you the power of Docker. In the sea of new technology, it can be hard to navigate the waters alone and tutorials such as this one can provide a helping hand. This is the Docker tutorial I wish I had when I was starting out. Hopefully it served its purpose of getting you excited about containers so that you no longer have to watch the action from the sides.

Below are a few additional resources that will be beneficial. For your next project, I strongly encourage you to use Docker. Keep in mind - practice makes perfect!

**Additional Resources**

- [Awesome Docker](https://github.com/veggiemonk/awesome-docker)
- [Hello Docker Workshop](http://docker.atbaker.me/)
- [Why Docker](https://blog.codeship.com/why-docker/)
- [Docker Weekly](https://www.docker.com/newsletter-subscription) and [archives](https://blog.docker.com/docker-weekly-archives/)
- [Codeship Blog](https://blog.codeship.com/)

Off you go, young padawan!

### Give Feedback

Now that the tutorial is over, it's my turn to ask questions. How did you like the tutorial? Did you find the tutorial to be a complete mess or did you have fun and learn something?

Send in your thoughts directly to [me](mailto:prakhar@prakhar.me) or just [create an issue](https://github.com/prakhar1989/docker-curriculum/issues/new). I'm on [Twitter](https://twitter.com/prakharsriv9), too, so if that's your deal, feel free to holler there!

I would totally love to hear about your experience with this tutorial. Give suggestions on how to make this better or let me know about my mistakes. I want this tutorial to be one of the best introductory tutorials on the web and I can't do it without your help.

___________
