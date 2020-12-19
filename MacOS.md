## 脚本可以干什么

1. 自动更新lxk0301的京东薅羊毛脚本，自动更新我的shell脚本。

2. 自动按设定天数删除旧日志。

3. 自动添加新的定时任务。

4. 自动删除失效的定时任务。

5. 自动按crontab.list设定的时间去跑各个薅羊毛脚本。

## 准备环境

1. 在[Node.js官网](https://nodejs.org/zh-cn/download)下载并安装Node.js长期支持版（已包括npm）；

2. 安装git、wget、curl、perl，可能某个软件已经集成在系统中。

## 流程

**以下全文均以此路径`/user/用户名/jd`进行举例，请自行修改为你自己的路径！**

**以下全文均以此路径`/user/用户名/jd`进行举例，请自行修改为你自己的路径！**

**以下全文均以此路径`/user/用户名/jd`进行举例，请自行修改为你自己的路径！**

**如需多账号并发，请在不同文件夹下分别按以下流程操作一遍，并且需要注意：目录虽然是分开的，但每套脚本下的`crontab.list`中，要把每一套脚本的定时任务都写一遍，即你几套账号，就要重复几次。记住，是每套脚本下的`crontab.list`全部要重复多次。**

1. 克隆本仓库

    如需以后从Github更新我的和lxk0301大佬的脚本：

    ```shell
    git clone -b v3 https://github.com/EvineDeng/jd-base /user/用户名/jd
    ```

    如需以后从Gitee更新我的和lxk0301大佬的脚本：

    ```shell
    git clone -b v3 https://gitee.com/evine/jd-base /user/用户名/jd
    ```

2. 复制并编辑自己的配置文件

    ```
    cd /user/用户名/jd
    cp sample/config.sh.sample config.sh         # 复制仓库下sample/config.sh.sample到主目录中，并命名为config.sh
    cp sample/computer.list.sample crontab.list  # 复制仓库下sample/computer.list.sample到主目录中，并命名为crontab.list
    ```
    
    其中`config.sh`是配置文件，`crontab.list`是定时任务清单。编辑这两个文件，如何编辑请查看两个文件内的注释。
    
    关于`crontab.list`，这里说明一下，除了那些本来就会准时运行的脚本外，如果还有一些脚本你不想随机延迟，要么在`config.sh`中`RandomDelay`不要赋值(所有任务都将不延迟执行)，要么参考下面 [如何手动运行脚本](MacOS#如何手动运行脚本) 部分，在`crontab.list`中不想被随机延迟运行的任务后面，添加上 `now`，比如：
    
    ```shell
    20 * * * * bash /user/用户名/jd/jd.sh jd_dreamFactory now
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
    cd /user/用户名/jd/scripts
    npm install || npm install --registry=https://registry.npm.taobao.org
    ```

4. 添加定时任务

   **请注意：以下命令会完整覆盖你当前用户的crontab清单，请务必先检查当前用户是否存在其他定时任务！！！**

    **请注意：以下命令会完整覆盖你当前用户的crontab清单，请务必先检查当前用户是否存在其他定时任务！！！**

    **请注意：以下命令会完整覆盖你当前用户的crontab清单，请务必先检查当前用户是否存在其他定时任务！！！**

    > **你可以先使用`crontab -l`命令检查当前用户的定时任务清单，如果有，请将这部分任务放在下面这个`crontab.list`文件中指定的位置（注意看注释的说明），然后再运行下面的命令，这样你原来的定时任务也就保留了。**
    
    > **如果以后你还要增加其他定时任务，也请加在这个文件最上方以后，再运行下面命令。如果不添加在这个文件中，那么脚本会在自动增删定时任务时以crontab.list中的清单覆盖掉你通过其他方式添加的定时任务。**

    ```shell
    cd /user/用户名/jd
    crontab crontab.list
    ```

    **如果你害怕你其他的任务被脚本覆盖，那么请将config.sh中的`AutoAddCron`和`AutoDelCron`都设置为`false`！！！这样脚本就不会自动增删任务了！！！**

    **如果你害怕你其他的任务被脚本覆盖，那么请将config.sh中的`AutoAddCron`和`AutoDelCron`都设置为`false`！！！这样脚本就不会自动增删任务了！！！**

    **如果你害怕你其他的任务被脚本覆盖，那么请将config.sh中的`AutoAddCron`和`AutoDelCron`都设置为`false`！！！这样脚本就不会自动增删任务了！！！**

5. 部署完成。

## 如何手动运行脚本

用法如下(其中`xxx`为lxk0301大佬的脚本名称)：

```
cd /user/用户名/jd
bash jd.sh xxx      # 如果设置了随机延迟并且当时时间不在0-2、30-31、59分内，将随机延迟一定秒数
bash jd.sh xxx now  # 无论是否设置了随机延迟，均立即运行
```

如果你没输lxk0301大佬的脚本名称也不要紧，`jd.sh`会提示你：

![PC1](Picture/PC1.png)

如果lxk0301脚本名不记得也不要紧，输错了也会提示你的：

![PC2](Picture/PC2.png)

## 如何更新配置文件

- `config.sh`改完保存好就行，其他啥也不用干，容器也无需重启什么的，改完以后，新的任务就以新配置运行了。

- `crontab.list`改完后要运行一下`docker exec -it crontab /user/用户名/jd/crontab.list`才会立即生效。