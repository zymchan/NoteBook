#### Python3环境下ride中文乱码解决办法
将**C:\Program Files\Python37\Lib\site-packages\robotide\contrib\testrunner\testrunnerplugin.py**文件中

> textctrl.AppendTextRaw(bytes(string, encoding['SYSTEM']))

改成：

> textctrl.AppendTextRaw(bytes(string, encoding['OUTPUT']))




#### Oracle连接报错
Oracle连接的时候报错如下：

> DatabaseError: DPI-1047: Cannot locate a 64-bit Oracle Client library: "The specified module could not be found"

解决办法： 将PL/SQL安装所需要的**instantclient**的目录加入环境变量