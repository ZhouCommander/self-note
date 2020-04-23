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

   - **WSGI 协议**

   ```
   不修改服务器和架构代码而确保可以在多个架构下运行web服务器。
   WSGI允许开发者将选择web框架和web服务器分开。可以混合匹配web服务器和web框架，选择一个适合的配对。
   ```


   - **网络字序节**

     ```
     字节序，字节的顺序，大于一个字节类型的数据在内存中的存放顺序。
     网络字节序是TCP/IP中规定好的一种数据表示格式，它与具体的CPU类型、操作系统等无关，从而保证数据在不同主机之间传输时能被正确解释。网络字节顺序采用big endian排序方式。
     big-endian：高位字节排放在内存的低地址端，低位字节排放在内存的高位置端
     传输次序：大端字节序
     ```

   - **tcp协议**

     ```
     tcp与udp的不同：
     1.面向连接
     2.有序数据传输
     3.重发丢失的数据包
     4.舍弃重复的数据包
     5.无差错的数据传输
     6.阻塞/流量控制

     三次握手和四次挥手：
     seq:序列号 建立连接
     ack:确认号 确认连接
     三次挥手：
     Client ->SYN seq=x-> Server
     Server ->ACK=1 SYN=1 ack=x+1 seq=y-> Client
     Client ->ACK=1 seq=x+1 ack=y+1 ->Server
     数据传输：
     Client ->ACK y+1-> Server
     Server ->ACK y+2-> Client
     四次挥手：
     Client(fin wait 1) ->FIN=1 seq=u-> Server
     Server(close wait) -> ACK=1 seq=v ack=u+1 -> Client(fin wait 1)
     Server(close wait) -> FIN=1 ACK=1 seq=w ack=u+1 -> Client(fin wait 2)
     Client(2 MSL wait) -> ACK=1 ack=w+1 seq=u+1 -> Server(close)

     TIME_WAIT的 2MSL 状态：
     2MSL 最大报文段生存时间 即两倍的 Maximum Segment Lifetime，指一个片段在网络中最大的存活时间。2MSL就是一个发送和一个回复所需的最大时间。

     如果建立连接只采用两次握手，会发生死锁。

     如果已经建立连接，但客户端突然出现故障，那么服务器不能一直等下去，白白浪费资源。服务器每收到一次客户端的请求后都会重新复位这个计时器，时间通常是设置为2小时，若两小时还没有收到客户端的任何数据，服务器就会发送一个探测报文段，以后每隔75秒钟发送一次。若一连发送10个探测报文仍然没反应，服务器就认为客户端出了故障，接着就关闭连接。

     在tcp/ip协议中的数据传输过程，数据被一步步封装，然后加入信息首部， 当传送到目的端时，被一步步解封，然后获取数据。任何发出的数据包都会被分配一个序列号，以便被请求的主机能够在特定的时间内对分配的这风格序列号进行确认。
     ```

- **udp协议**

  ​