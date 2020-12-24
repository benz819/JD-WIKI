## 申明申明申明申明申明申明申明申明申明申明申明

1. **经测试，受到手机电量管理策略的影响，安卓下的定时任务似乎有点抽风，有时候能准时运行，有时候又不能准时运行。**

2. **如果被手机杀掉了进程，那么所有定时任务都不会执行了。**

3. **每次重启手机或重启`Termux`后需要重新手动启动crond程序。**

4. **绝大部分手机在一段时间不使用以后，将进行低功耗状态，这个状态下，所有程序都将受限，那么自然定时任务也就不会运行了。这种情况在晚上睡觉时尤其明显。**

5. **建议安卓运行脚本只做为其他脚本运行方式的一种补充，比如在有类似家电星推官时，可以在手机上运行以达到`手动准时运行`的效果。**

6. **v3版已经可以正常在外置存储运行了。**

## 脚本可以干什么

1. 自动更新lxk0301的京东薅羊毛脚本，自动更新我的shell脚本。

2. 自动按设定天数删除旧日志。

3. 自动添加新的定时任务。

4. 自动删除失效的定时任务。

5. 自动按crontab.list设定的时间去跑各个薅羊毛脚本。

## 准备工作

1. 想办法安装好谷歌服务框架，注意：需要科学上网条件。

2. 从谷歌商店搜索并安装 `Termux`。

3. 从 [这里](https://www.sqlsec.com/2018/05/termux.html) 学习 `Termux` 的基础用法，这其中介绍的 `termux-ohmyzsh` 一定要装，能显著减少手机上的输入活动。另外，也建议按照该教程进行：切换为国内源、美化终端、允许访问手机外置存储等操作，其他部分也建议多看多学。

## 安装依赖

切换为国内安装源后，在`Termux`中输入：
```shell
pkg upgrade
pkg install git perl nodejs-lts wget curl nano cronie moreutils
```

针对`Termux`输入麻烦，多多使用`Tab`键自动补全是个好习惯。`termux-ohmyzsh`也是一个神器，启用后可以帮助输入。

## 一些重要的前提

**`Termux`的家目录为：`/data/data/com.termux/files/home`，一般家目录用`~`这个符号代替，这个家目录位于手机的内置存储中。（请注意，日常在终端操作，家目录写为`~`没有问题，但请在crontab.list中写作完整的路径，不要写作`~`）**

按照上面`Termux`[教程](https://www.sqlsec.com/2018/05/termux.html) 中允许访问手机外置存储的操作后，家目录下会有一个`storage`的软连接（`~/storage`），`storage`中有个`shared`文件夹（`~/storage/shared`），连接的就是手机的外置存储。

v3版脚本已经可以正常在外置存储运行了，这样查看日志就方便多了。

## 操作流程

**如需多账号并发，请在不同文件夹下分别按以下流程操作一遍，并且需要注意：目录虽然是分开的，但每套脚本下的`crontab.list`中，要把每一套脚本的定时任务都写一遍，即你几套账号，就要重复几次。记住，是每套脚本下的`crontab.list`全部要重复多次。**

**看不懂这句话的小白就别折腾多账号并发了...**

1. 克隆本仓库

    **按上述`Termux`[教程](https://www.sqlsec.com/2018/05/termux.html) 中允许访问手机外置存储的操作后，下面的命令将在外置存储根目录创建一个jd文件夹保存本仓库脚本。**

    如需以后从Github更新我的和lxk0301大佬的脚本：

    ```shell
    git clone -b v3 https://github.com/EvineDeng/jd-base ~/storage/shared/jd
    ```

    如需以后从Gitee更新我的和lxk0301大佬的脚本：

    ```shell
    git clone -b v3 https://gitee.com/evine/jd-base ~/storage/shared/jd
    ```

2. 复制并编辑自己的配置文件

    ```
    cd ~/storage/shared/jd

    # 创建一个配置文件保存目录
    mkdir config

    # 复制仓库下sample/config.sh.sample到config目录中，并命名为config.sh
    cp sample/config.sh.sample config/config.sh

    # 复制仓库下sample/termux.list.sample到config目录中，并命名为crontab.list
    cp sample/termux.list.sample config/crontab.list
    
    # 编辑刚刚复制后的两个文件，Ctr + O保存，Ctrl + X退出
    nano config/config.sh
    nano config/crontab.list    # 请注意crontab.list中的路径中家目录不要用~代替，必须是完整的路径 
    ```

    1. 你可以按上面方式直接在nano终端编辑器中修改参数，但可通过其他途径将必要的信息复制过来粘贴；

    2. 也可以用cp命令将`git_pull.sh`复制到外置存储卡，用其他可视化文件编辑器修改好后，再复制回来；

    3. 甚至还可以参考上述`Termux`[教程](https://www.sqlsec.com/2018/05/termux.html)，在运行`sshd`服务程序后，通过局域网内的电脑，使用WinSCP软件连接手机进行修改。
    
    > 其中`config.sh`是配置文件，`crontab.list`是定时任务清单。如何编辑请查看两个文件内的注释。
    
    > 关于`crontab.list`，这里说明一下，除了那些本来就会准时运行的脚本外，如果还有一些脚本你不想随机延迟，要么在`config.sh`中`RandomDelay`不要赋值(所有任务都将不延迟执行)，要么参考下面 [如何手动运行脚本](Android#如何手动运行脚本) 部分，在`crontab.list`中不想被随机延迟运行的任务后面，添加上 `now`，比如：
    
    ```shell
    20 * * * * bash /data/data/com.termux/files/home/storage/shared/jd/jd.sh jd_dreamFactory now
    ```

3. 初始化

    **在首次编辑好`config.sh`和`crontab.list`后，请务必手动运行一次`git_pull.sh`，不仅是为检查错误，也是为了运行一次`npm install`用以安装js指定的依赖。**

    ```shell
    bash git_pull.sh
    ```

    **针对首次运行`git_pull.sh`**，出现类似以下字样才表示`npm install`运行成功：
    ```
    audited 205 packages in 3.784s

    11 packages are looking for funding
    run `npm fund` for details

    found 0 vulnerabilities
    ```

    如果`npm install`失败，请尝试手动运行，可按如下操作，如果失败，可运行多次：

    ```shell
    cd ~/storage/shared/jd/scripts
    npm install || npm install --registry=https://registry.npm.taobao.org
    ```

4. 添加定时任务

- 在添加定时任务之前，请先熟悉一下手机上cronie这个软件的用法（你也可以随时输入`crond -h`查看此帮助）：

    ```shell
    ~/storage/shared/jd/shell/ crond -h
    Usage:
    crond [options]

    Options:
    -h         print this message
    -i         deamon runs without inotify support
    -m <comm>  off, or specify preferred client for sending mails
    -n         run in foreground
    -p         permit any crontab
    -P         use PATH="/data/data/com.termux/files/usr/bin"
    -s         log into syslog instead of sending mails
    -V         print version and exit
    -x <flag>  print debug information

    Debugging flags are: ext,sch,proc,pars,load,misc,test,bit

    ```
    根据帮助文档，如果想要以`deamon`形式启动`cronie`，那么可以输入：
    ```shell
    crond -ipP
    ```

    **请注意：每次重启手机或重启`Termux`后需要重新输入上述命令。**

    如果输入上述命令后显示类似以下内容的错误，那么表示cronie已经启动好了，无需再次启动：
    ```shell
    crond: can't lock /data/data/com.termux/files/usr/var/run/crond.pid, otherpid may be 3087: Try again
    ```

- 启动好cronie后，再执行以下命令：

    ```shell
    cd ~/storage/shared/jd
    crontab config/crontab.list
    ```

5. 部署完成。在有定时任务运行过后，日志文件会在`~/storage/shared/jd/log`中。

## 如何更新配置文件

`config.sh`和`crontab.list`两个文件都一样，改完保存好就行，其他啥也不用干，改完以后，新的任务就以新配置运行了。

## 如何添加除lxk0301大佬以外的脚本

本环境基于node，所以也只能跑js脚本。你可以把你的后缀为`.js`的脚本放在`~/storage/shared/jd/scripts`下或`~/storage/shared/jd/config`下。比如你在`~/storage/shared/jd/config`下放了个`test.js`，可以在你的`crontab.list`中添加如下的定时任务：

```shell
15 10 * * * bash /data/data/com.termux/files/home/storage/shared/jd/jd.sh test     # 如果不需要准时运行或RandemDelay未设置
15 10 * * * bash /data/data/com.termux/files/home/storage/shared/jd/jd.sh test now # 如果设置了RandemDelay但又需要它准时运行
```
程序会先检测`~/storage/shared/jd/scripts`下有没有`test.js`，再检测`~/storage/shared/jd/config`有没有，以先检测到的为准。假如两个文件夹下都没有，就会报错。

然后运行一下`crontab ~/storage/shared/jd/config/crontab.list`更新定时任务即可。

## 如何手动运行脚本

1. 手动 git pull 更新脚本

    ```shell
    cd ~/storage/shared/jd
    bash git_pull.sh
    ```

2. 手动删除指定时间以前的旧日志

    ```shell
    cd ~/storage/shared/jd
    bash rm_log.sh
    ```

3. 手动导出所有互助码

    ```shell
    cd ~/storage/shared/jd
    bash export_sharecodes.sh
    ```

4. 手动执行薅羊毛脚本，用法如下(其中`xxx`为lxk0301大佬的脚本名称)，不支持直接以`node xxx.js`命令运行：

    ```
    cd ~/storage/shared/jd
    bash jd.sh xxx      # 如果设置了随机延迟并且当时时间不在0-2、30-31、59分内，将随机延迟一定秒数
    bash jd.sh xxx now  # 无论是否设置了随机延迟，均立即运行
    ```

    如果你没输lxk0301大佬的脚本名称也不要紧，`jd.sh`会提示你：

    ![PC1](Picture/PC1.png)

    如果lxk0301脚本名不记得也不要紧，输错了也会提示你的：

    ![PC2](Picture/PC2.png)

## 划重点：电量优化策略

这就是手机上运行定时任务的难点了，请各位各显神通，授予`Termux`无限制的后台策略，常见的有：允许常驻后台，允许后台联网，无限制的电量优化等等。至于做了这些操作以后，`Termux`是否仍然偶尔抽风，那我就不得而知了。

但即使这样，遇到特殊情况时，不仍然可以进入`~/storage/shared/jd/scripts/`后手动运行js脚本吗？

如果有个旧的安卓手机，是不是可以考虑一直充电放家中，无限制地运行此脚本？

## 补充说明

手机上的`git`在运行时总会弹出一个警告如下：

```shell
warning: Pulling without specifying how to reconcile divergent branches is discouraged. You can squelch this message by running one of the following commands sometime before your next pull:

git config pull.rebase false  # merge (the default strategy)
git config pull.rebase true   # rebase
git config pull.ff only       # fast-forward only
    
You can replace "git config" with "git config --global" to set a default preference for all repositories. You can also pass --rebase, --no-rebase, or --ff-only on the command line to override the configured default perinvocation.
```

如果想要永久消除这个提示，按它说明的操作方式输入一下命令即可：

```shell
git config --global pull.rebase false
```