> 查询所有的命令
```bash
robot --help
```

>常用的执行命令
```bash
# 执行指定目录下所有suite
robot D:\test_folder
# 执行指定的suite
robot D:\test_folder\tests.robot
# 执行指定的Case
robot --testcase1 in D:\test_folder\tests.robot
# 执行指定的suite
robot D:\test_folder\tests.robot
# 测试结果输出到指定目录
robot --outputdir D:\reportFolder tests

# 执行指定Tag的用例
robot --include tag1 --exclude tag2 tests
# 参数化执行用例
robot -v para1:value1 -v para2:value2 tests

```


>失败重新执行（rerun）
```bash
# first execute all tests
robot --output output1.xml tests
# then re-execute failing
robot --rerunfailed output1.xml --output output2.xml tests
# then re-execute failing again
robot --rerunfailed output2.xml --output output3.xml tests
# finally merge results
rebot --output output.xml --merge output*.xml

```