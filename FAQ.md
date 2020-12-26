
## 配置文件

- 互助码填法

    互助码是填在My系列变量中的，ForOther系统变量中只要填入My系列的变量名即可，按注释中的例子拼接，如下图所示：

![Sharecodes](Picture/Sharecodes.png)
    
## 容器时间

据反馈，arm/v7平台上的Docker容器，时间有问题，怎么改都只能是UTC时间。在这种情况下，直接用你的arm/v7机器，按照我的Dockerfile自行构建一个镜像，那么时间就可以是北京时间。构建流程：

1. cd 至你的一个文件夹下，然后克隆仓库：

    ```shell
    git clone https://github.com/EvineDeng/jd-base
    ```

2. 进入刚刚克隆的文件夹内：

     ```shell
     cd jd-base
     ```

3. 构建镜像，注意最后有个点，构建好的镜像名称为`jd-base`：

    ```shell
    docker build -t jd-base:latest -f Docker/Dockerfile .
    ```

4. 按照你之前创建容器的命令，或者参考 [[Docker]] 教程中的命令，创建容器即可，不过要将原来我的镜像 `evinedeng/jd-base:latest` 切换为你自己刚刚构建的镜像 `jd-base:latest` 。

5. 我如果在 [release](https://github.com/EvineDeng/jd-base/releases) 页面发布了新的 release，那么就意味着你也可以重新构建你的容器了。记得先删除原来的容器和原来的镜像。

## 日志报错

- 日志报以下错误：
    ```
    TypeError: (intermediate value)(...).catch(...).finally is not a function
    at Object. (/home/bobotoy/jd/scripts/jd_bean_sign.js:52:13)
    at Module._compile (module.js:652:30)
    at Object.Module._extensions..js (module.js:663:10)
    at Module.load (module.js:565:32)
    at tryModuleLoad (module.js:505:12)
    at Function.Module._load (module.js:497:3)
    at Function.Module.runMain (module.js:693:10)
    at startup (bootstrap_node.js:188:16)
    at bootstrap_node.js:609:3
    ```
    首先，需要保证Node.js大版本>=10，其次，在>=10的某些小版本也有此问题（你可以使用`node -v`或`nodejs -v`查看Node.js的版本），问题及详细的解决措施见[nodejs/help#1877](https://github.com/nodejs/help/issues/1877)。
