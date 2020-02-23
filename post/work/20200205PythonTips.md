## Python Learning Tips

### 如何运行python模块

[运行python脚本有两种方式](http://www.pythondoc.com/pythontutorial3/modules.html)：

- method one 
  

当你使用以下方式运行 Python 模块时，模块中的代码便会被执行:

python fibo.py <arguments>
模块中的代码会被执行，就像导入它一样，不过此时 __name__ 被设置为 "__main__"。这相当于，如果你在模块后加入如下代码:

if __name__ == "__main__":
    import sys
    fib(int(sys.argv[1]))
就可以让此文件像作为模块导入时一样作为脚本执行。此代码只有在模块作为 “main” 文件执行时才被调用:

$ python fibo.py 50
1 1 2 3 5 8 13 21 34
如果模块被导入，不会执行这段代码:
```python
>>> import fibo
>>>
```


这通常用来为模块提供一个便于测试的用户接口（将模块作为脚本执行测试需求）。

  How do I install a Python module for use on Linux systems at SEAS?
This article assumes you are logged into a CETS managed Linux machine (Eniac, a lab machine, graduate workstation, etc).

These first two steps only need to be done once.
Append your .bash_profile so PYTHONPATH includes ~/lib/python2.7/site-packages/ like this:

```bash
echo export PYTHONPATH="$PYTHONPATH:~/lib/python2.7/site-packages/" >> ~/.bash_profile
Run this command to update the PYTHONPATH for the current session:

source ~/.bash_profile
```

Installing modules via pip to your home directory
Once you have configured your PYTHONPATH as described above, you can install packages locally by adding the --user flag when calling pip:

```bash
pip install --user PACKAGE_NAME_HERE
```
Or, if you have a requirements.txt file that lists necessary dependencies:

```bash
pip install --user -r requirements.txt
```
Installing via modules via setup.py to your home directory
Download and untar or unzip the module you would like to install.

cd into the module directory that contains setup.py and run the install:

```bash
python setup.py install --prefix=~
```



- method two

python -m test,模块运行；区别与直接运行，直接启动是把test.py文件，所在的目录放到了sys.path属性中。
模块启动是把你输入命令的目录（也就是当前路径），放到了sys.path属性中.
在没有包之间导入关系时，两者的效果一样，[区别情况](https://www.cnblogs.com/xueweihan/p/5118222.html)
```python
# 比如说
python -m test 
与
python test.py
效果一样。

```

出于性能考虑，每个模块在每个解释器会话中只导入一遍。因此，如果你修改了你的模块，需要重启解释器；
或者，如果你就是想交互式的测试这么一个模块，可以用 imp.reload() 重新加载，例如 import imp; imp.reload(modulename)。


### [Python传入参数的几种方法](https://cloud.tencent.com/developer/article/1567332)


- 位置参数
位置参数是最简单的传入参数的方式，在其它的语言中也常常被使用
```python
#演示一：

def func(a, b):
    print(a+b)

func(1, 2)  #3
#演示二：

def power(x, n):
    s = 1
    while(n > 0):
        n -= 1
        s *= n
    return s

power(2, 3) #8
```

- 默认参数 
定义默认参数要牢记一点：默认参数必须指向不变对象！

```python
#以下这个函数如果被多次调用会在默认添加多个END字符串
def add_end(l = []):
    l.append('END')
    return l
#为了避免这个问题，应该把传入的默认参数设置为不可变的
def add_end(l = None):
    l = []
    l.append('END')
    return l

```


- 可变参数
可变参数就是允许在调用参数的时候传入多个（≥0个）参数（类似于列表、字典）

```python

#传入一个列表，严格地说这不是可变参数
def calc(l):
    sum = 0
    for n in l:
        sum += n
    return sum

>>> calc([1,2,3])
7
#这才是可变参数，虽然在使用上和列表没有区别，但是参数nums接收到的是一个tuple（这些参数在传入时被自动组组装为一个元祖）
def calc(*nums):
    sum = 0
    for n in nums:
        sum += n
    return sum

>>> calc(1,2,3)
7

>>> my_ls = [1,2,3]
>>> calc(*my_ls)
7

```

- 关键字参数
可变参数允许传入0个～多个参数，而关键字参数允许在调用时以字典形式传入0个或多个参数（注意区别，一个是字典一个是列表）；在传递参数时用等号（=）连接键和值

```python

#用两个星号表示关键字参数
def person_info(name, age, **kw):
    print("name", name, "age", age, "other", kw)

>>> person_info("Xiaoming", 12)
name Xiaoming age 12 other{}
>>> person_info("Dahuang", 35, city = "Beijing")
name Dahuang age 35 other {'city':'Beijing'}

```


- 命名关键字参数
命名关键字参数在关键字参数的基础上限制传入的的关键字的变量名

和普通关键字参数不同，命名关键字参数需要一个用来区分的分隔符*，它后面的参数被认为是命名关键字参数

```python

#这里星号分割符后面的city、job是命名关键字参数
person_info(name, age, *, city, job):
    print(name, age, city, job)
```
person_info("Alex", 17, city = "Beijing", job = "Engineer")
Alex 17 Beijing Engineer    #看来这里不再被自动组装为字典
不过也有例外，如果参数中已经有一个可变参数的话，前面讲的星号分割符就不要写了（其实星号是写给Python解释器看的，如果一个星号也没有的话就无法区分命名关键字参数和位置参数了，而如果有一个星号即使来自变长参数就可以区分开来）


- 参数组合
总结一下，在Python中一种可以使用5中传递参数的方式（位置参数、默认参数、变长参数、关键字参数、命名关键字参数）

注意，这些参数在书写时要遵循一定的顺序即：位置参数、默认参数、变长参数、关键字参数、命名关键字参数（和本文的行文顺序一致）

这里简单举两个栗子

def f1(a, b, c=0, *args, **kw):
    print("a = ", a, "b = ", b, "args = ", args, "kw = ",kw)
def f2(a, b, c=0, *, d, **kw):
    print("a = ", a, "b = ", b, "c = ", c, "d = ", d, "kw = ", kw)

>>> f1(1, 2)
a = 1 b = 2 c = 0 args =() kw = {}
>>> f1(1, 2, c=3)
a = 1 b = 2 c = 3 args = () kw = {}
>>> f1(1, 2, 3, 'a', 'b')
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
>>> f1(1, 2, 3, 'a', 'b', x = 99)
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x':99}
>>> f2(1, 2, d = 99, ext = None)
a = 1 b =2 c = 0 d = 99 kw = {'ext':None}

- 参数注意
关于Python参数传递，有以下几点提请注意：

1）参数的传递是通过自动将对象赋值给本地变量名来实现的 
 函数参数在实际中只是Python赋值的另一个实例而已，因为引用可以是以指针的形式来实现的，所有的参数实际上都是通过指针进行传递的，作为参数被传递的对象从来不自动拷贝

2）在函数内部的参数名的赋值不会影响调用者 
 在函数运行时，在函数头部的参数名时一个新的、本地的变量名，这个变量名是在函数的本地作用域内的，函数参数名和调用者作用域中的变量是没有区别的

3）改变函数的可变对象参数的值也许会对调用者有影响 
 换句话说，因为参数是简单地赋值给传入的对象，函数就能够就地改变传入的可变对象，因此其结果会影响调用者；可变参数对函数来说可以做输入和输出的

Python的通过赋值进行传递的机制与C++的引用参数选项不完全相同，但是实际中，它与C语言的参数传递模型相当类似：

1）不可变参数“通过值”进行传递 
 像整数和字符串这样的对象是不可变对象，它们通过对象引用而不是拷贝进行传递的，但是因为无论如何都不可能在原处改变不可变对象，实际的效果就很像创建了一份拷贝

2）可变对象是通过“指针”进行传递的 
 列表和字典这样的对象也是通过对象引用进行传递的，这一点与C语言使用指针传递数组很相似，可变对象能够在函数内部进行原处的改变，这一点和C数组很像



### [decorator包装器](https://github.com/GrahamDumpleton/wrapt/blob/develop/blog/01-how-you-implemented-your-python-decorator-is-wrong.md)