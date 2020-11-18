## first_run.sh

- Failed connect to raw.githubusercontent.com:443; 拒绝连接

    要么自己想办法科学上网，并且让Github通过代理访问；要么自己手动下载first_run.sh这个文件放在该目录下运行。

## git_pull.sh

- 互助码填法

    互助码是填在My系列变量中的，ForOther系统变量中只要填入My系列的变量名即可，按注释中的例子拼接。
    
- 修改了配置但日志显示修改的内容未生效

    修改了git_pull.sh想要立马生效，你要手动运行一次它。

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
