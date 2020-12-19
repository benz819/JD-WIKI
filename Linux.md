## 脚本可以干什么

1. 自动更新lxk0301的京东薅羊毛脚本，自动更新我的shell脚本。

2. 自动按设定天数删除旧日志。

3. 自动添加新的定时任务。

4. 自动删除失效的定时任务。

5. 自动按crontab.list设定的时间去跑各个薅羊毛脚本。

## 准备环境

- debian/ubuntu/armbian/OpenMediaVault，以及其他debian系：
    ```shell
    apt update && apt install -y git wget curl nodejs npm perl
    ```
- CentOS/RedHat/Fedora等红帽系
    ```shell
    yum update && yum install git wget curl perl
    ```
    如果安装源中有nodejs和npm，也安装好，否则请访问 [Node.js官网](https://nodejs.org/zh-cn/download/) 或者 [nodesource@github](https://github.com/nodesource/distributions) 查看如何安装。

- OpenWrt， **需要添加官方软件源，** 如果某个软件包已集成在固件中，则可跳过安装。如果你会编译，可以把下面这些包直接编译在固件中。
    ```shell
    opkg update && opkg install git git-http wget curl node node-npm perl
    ```
    **声明：OpenWrt环境千差万别，不保证一定可用，需要根据自己的环境来配置，如果OpenWrt安装了Docker，也可以使用Docker的方法。Rom小于256M就不要安装了，你空间不够。**

*注：不同系统的包名不一定一样，需保证 node 大版本 >=10，安装好后使用`node -v`或`nodejs -v`命令可查看版本。*

## 流程

**以下全文均以此路径`/home/myid/jd`进行举例，请自行修改为你自己的路径！**

**以下全文均以此路径`/home/myid/jd`进行举例，请自行修改为你自己的路径！**

**以下全文均以此路径`/home/myid/jd`进行举例，请自行修改为你自己的路径！**

1. 克隆本仓库

    如需以后从Github更新我的和lxk0301大佬的脚本：

    ```shell
    git clone -b v3 https://github.com/EvineDeng/jd-base /home/myid/jd
    ```

    如需以后从Gitee更新我的和lxk0301大佬的脚本：

    ```shell
    git clone -b v3 https://gitee.com/evine/jd-base /home/myid/jd
    ```

2. 复制并编辑自己的配置文件

    ```
    cd /home/myid/jd
    cp sample/config.sh.sample config.sh         # 复制仓库下sample/config.sh.sample到主目录中，并命名为config.sh
    cp sample/computer.list.sample crontab.list  # 复制仓库下sample/computer.list.sample到主目录中，并命名为crontab.list
    ```
    
    其中`config.sh`是配置文件，`crontab.list`是定时任务清单。编辑这个文件，如何编辑请查看两个文件内的注释。

    - *注1：如果在windows下编辑，请使用WinSCP自带编辑器，或 notepad++、VS Code、Sublime Text 3等软件，请不要使用记事本。*

    - *注2：如何修改请仔细阅读文件中的注释部分。*

    - *注3：如果在WinSCP中看不见文件或看见了但不是最新的文件，请点击鼠标右键-刷新来刷新文件清单。*
    
    关于`crontab.list`，这里说明一下，除了那些本来就会准时运行的脚本外，如果还有一些脚本你不想随机延迟，要么在`config.sh`中`RandomDelay`不要赋值(所有任务都将不延迟执行)，要么参考下面 [如何手动运行脚本](Linux#如何手动运行脚本) 部分，在`crontab.list`中不想被随机延迟运行的任务后面，添加上 `now`，比如：
    
    ```shell
    20 * * * * bash /home/myid/jd/jd.sh jd_dreamFactory now
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
    cd /home/myid/jd/scripts
    npm install || npm install --registry=https://registry.npm.taobao.org
    ```

4. 添加定时任务

   **请注意：以下命令会完整覆盖你当前用户的crontab清单，请务必先检查当前用户是否存在其他定时任务！！！**

    **请注意：以下命令会完整覆盖你当前用户的crontab清单，请务必先检查当前用户是否存在其他定时任务！！！**

    **请注意：以下命令会完整覆盖你当前用户的crontab清单，请务必先检查当前用户是否存在其他定时任务！！！**

    > **你可以先使用`crontab -l`命令检查当前用户的定时任务清单，如果有，请将这部分任务放在下面这个`crontab.list`文件中指定的位置（注意看注释的说明），然后再运行下面的命令，这样你原来的定时任务也就保留了。**
    
    > **如果以后你还要增加其他定时任务，也请加在这个文件最上方以后，再运行下面命令。如果不添加在这个文件中，那么脚本会在自动增删定时任务时以crontab.list中的清单覆盖掉你通过其他方式添加的定时任务。**

    ```shell
    cd /home/myid/jd
    crontab crontab.list
    ```

    **如果你害怕你其他的任务被脚本覆盖，那么请将config.sh中的`AutoAddCron`和`AutoDelCron`都设置为`false`！！！这样脚本就不会自动增删任务了！！！**

    **如果你害怕你其他的任务被脚本覆盖，那么请将config.sh中的`AutoAddCron`和`AutoDelCron`都设置为`false`！！！这样脚本就不会自动增删任务了！！！**

    **如果你害怕你其他的任务被脚本覆盖，那么请将config.sh中的`AutoAddCron`和`AutoDelCron`都设置为`false`！！！这样脚本就不会自动增删任务了！！！**

5. 部署完成。

## 如何手动运行脚本

用法如下(其中`xxx`为lxk0301大佬的脚本名称)：

```
cd /home/myid/jd
bash jd.sh xxx      # 如果设置了随机延迟并且当时时间不在0-2、30-31、59分内，将随机延迟一定秒数
bash jd.sh xxx now  # 无论是否设置了随机延迟，均立即运行
```

如果你没输lxk0301大佬的脚本名称也不要紧，`jd.sh`会提示你：

![PC1](Picture/PC1.png)

如果lxk0301脚本名不记得也不要紧，输错了也会提示你的：

![PC2](Picture/PC2.png)