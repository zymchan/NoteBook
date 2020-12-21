
##### yield 的作用就是把一个函数变成一个generator，返回一个 iterable 对象。函数在执行的时候会在遇到yield的时候将值直接返回出来，供外部使用，当迭代器再次调用函数返回的对象的时候，函数会继续从yield后面的代码开始执行并开始下一轮的迭代。


使用举例：
```python
def fab(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b  # 使用 yield
        a, b = b, a + b
        n = n + 1
        print("n is %d" %n)
i = 1
for n in fab(3):
    print("第%d次迭代开始" %i)
    print("fab return: %d" %n )
    print("第%d次迭代完成" %i)
    i=i+1
```
输出结果
```
第1次迭代开始
fab return: 1
第1次迭代完成
n is 1
第2次迭代开始
fab return: 1
第2次迭代完成
n is 2
第3次迭代开始
fab return: 2
第3次迭代完成
n is 3
```
