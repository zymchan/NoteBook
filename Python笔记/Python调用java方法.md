>先需要安装[JPype](https://pypi.org/project/JPype1/)

pip 安装
```
pip install  JPype1
```
安装的时候可能会报错ssl的连接失败，用如下的命令安装
```
pip install --trusted-host=pypi.python.org --trusted-host=pypi.org --trusted-host=files.pythonhosted.org JPype1
```

> 测试代码
```python
from jpype import *

startJVM(getDefaultJVMPath(), "-ea")
java.lang.System.out.println("Hello World")
shutdownJVM()
```

> 引用jar包的方法


在com目录下新建文件Test.java
```java
package com; 

public class Test { 

    public String run(String str){
        return str; 
        }        
 }
```

编译
```
javac Test.java
```


打包
```
jar cvf test.jar com
```


python调用
```python
from jpype import *

jarpath = os.path.join(os.path.abspath('.'), 'libs/test.jar') 
jpype.startJVM(jpype.getDefaultJVMPath(), "-ea", "-Djava.class.path=%s" % jarpath) 
Test = jpype.JClass('com.Test') 
# 或者通过JPackage引用Test类 
# com = jpype.JPackage('com') 
# Test = com.Test 
t = Test() 
res = t.run("a")
print(res)
jpype.shutdownJVM()


```