## 互联网/IT 入门知识 collection

### 目录

- **语言**
- **网络**
- **系统**
- **数据结构与算法**

### 正文

1. ### 语言

   **Python：**

   - **闭包与装饰器**

     ```python
     """闭包是回调式异步编程和函数式编程风格的基础"""
     def make_averager():
         series = []
         def averager(new_value):
           series.append(new_value)
           total = sum(series)
           return total/len(series)
        	return averager
     闭包的延迟绑定：
     这里的series作为make_averager函数的局部变量，会随着return的运行而失去作用域。虽然如此，但在闭包中，series可作为自由变量继续averager中的操作。所以，闭包时一种函数，会保留定义函数时存在的自由变量的绑定，这样调用函数时，虽然定义作用域不可用，但仍然使用那些绑定。
     nolocal声明：
     对于不可变类型变量，引入nolocal声明，将变量标记为自由变量

     """函数装饰器在导入模块时立即执行，而被装饰的函数旨在明确调用时运行。"""
     装饰器通常在一个模块中定义，然后应用到其他模块的函数中。
     register装饰器返回的函数与通过参数传入的相同，这里重点运用在python web框架里，把函数添加到某种中央注册处，集中管理集中调用。
     ```

   - **property属性**

     ```python
     定义：一种修饰实例方法的特殊属性，有装饰器和类属性两种变换形态
     功能：类似rest接口风格的一种修饰属性
     风格：装饰器 & 类属性
     装饰器：
     claclass Goods:
         """python3中默认继承object类
             以python2、3执行此程序的结果不同，因为只有在python3中才有@xxx.setter  @xxx.deleter
         """
         @property
         def price(self):  ## 查询
             print('@property')

         @price.setter  
         def price(self, value):  ## 修改
             print('@price.setter')

         @price.deleter
         def price(self):  ## 删除
             print('@price.deleter')

     obj = Goods()
     obj.price          # 自动执行 @property 修饰的 price 方法，并获取方法的返回值
     obj.price = 123    # 自动执行 @price.setter 修饰的 price 方法，并将  123 赋值给方法的参数
     del obj.price      # 自动执行 @price.deleter 修饰的 price 方法ss Goods

     类属性：
     class Foo(object):
         def get_bar(self):
             print("getter...")
             return 'laowang'

         def set_bar(self, value): 
             """必须两个参数"""
             print("setter...")
             return 'set value' + value

         def del_bar(self):
             print("deleter...")
             return 'laowang'

         BAR = property(get_bar, set_bar, del_bar, "description...")
     ```

   - with

     ```
     with操作，实现原理来自于上下文管理器，替代了try/finally，实现了文本的读取与清除。
     ```


   ​

2. ### 网络

   - WSGI 协议

     ```
     不修改服务器和架构代码而确保可以在多个架构下运行web服务器。
     WSGI允许开发者将选择web框架和web服务器分开。可以混合匹配web服务器和web框架，选择一个适合的配对。
     ```

     ​


