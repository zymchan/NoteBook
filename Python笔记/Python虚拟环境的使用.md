###  Step 1. 安装virtualenvwrapper

windows系统
```dos
pip install virtualenvwrapper-win
```
Linux系统
```dos
pip install virtualenvwrapper
```
***
###  Step 2. 指定WORKON_HOME

在系统环境变量新建系统变量**WORKON_HOME**，值指定为自己所创建的文件夹即可，如，d:/envs。
***

###  Step 3. 创建虚拟环境
直接使用**mkvirtualenv EnvName**,即可创建名字为EnvName的虚拟环境，此时在**WORKON_HOME**对应目录会生成对应环境的文件夹
```dos
C:\Users\zouyiming>mkvirtualenv EnvName1
created virtual environment CPython3.7.7.final.0-64 in 4730ms
  creator CPython3Windows(dest=D:\Envs\EnvName1, clear=False, global=False)
  seeder FromAppData(download=False, pip=latest, setuptools=latest, wheel=latest, via=copy, app_data_dir=C:\Users\zouyiming\AppData\Local\pypa\virtualenv\seed-app-data\v1.0.1)
  activators BashActivator,BatchActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator

(EnvName1) C:\Users\zouyiming>
```
**(EnvName1) C:\Users\zouyiming>** 代表已经进入了虚拟环境

创建指定python版本环境的虚拟环境
```dos
mkvirtualenv --python=D:\Python\Python37\python.exe python37
```

退出dos窗口后，想要再次进入虚拟环境只需 **workon EnvName1**


###  pycharm编译器指定虚拟环境
虚拟环境下的所有安装都是针对虚拟环境的，如果pycharm打开项目需要指定对应的虚拟环境的编译器才能获取项目对应的依赖。

>File-->Settings-->Project-->Project Interpreter-->找到WORKON_HOME目录下对应的虚拟环境下的python.exe


###  将虚拟环境下的依赖导出到requirement.txt
在虚拟环境中
```dos
pip freeze > requirements.txt
```

###  安装requirement.txt中的依赖
进入虚拟环境后
```dos
pip install -r requirements.txt
```