#### 域名服务器与区域
存储域命名空间信息的程序叫做**名称服务器(nameserver)**。名称服务器通常只拥有域名空间某一部分的完整信息，这一部分我们称为**区域(zone)**，区域的内容是从文件或另一个名称服务器加载而来。加载过后，这个名称服务器便可宣称对该区域具有**权威(authority)**。一个名称服务器可以同时对多个区域具有权威。 


#### 解析过程

##### 解析器：
在BIND中，解析器时链接到诸如ssh和ftp等程序的一组库例程(library routine)。解析器的功能一般非常简单： 收集查询命令，向名称服务器发送查询并等待应答，如果没有应答就再次发送查询。这就是解析器的全部任务了，具体的解析功能完全依赖于名称服务器。

##### 解析：
名称服务器不仅能提供以自己为权威的区域的数据，还能在域名空间中搜索找到不以自己为权威的区域的数据。这个过程被称为**名称解析(name resolution)**，简称解析。

由于域名称空间是一棵倒置的树，因此只要名称服务知道root名称服务器的域名和地址，就可以向root名称服务器查询域命名空间中的任何域名，然后root名称服务器会以自己的方式代为解析域名。

##### root 名称服务器： 
root名称服务器知道每个顶级区域的权威名称服务器的位置。 事实上，有些root名称服务器本身就是通用顶级区域的权威。） 对于查询的任何域名，root名称服务器都可以为其提供该域名所在顶级区域的那些权威名称服务器的名称和地址。 然后，顶级名称服务器会提供该域名所在二级区域中权威服务器的列表。每个被查询的名称服务器要么直接给出答案，要么提供如何进一步寻找答案的信息。很显然，root服务器对于解析来说非常重要。正因为它是如此的重要，所以DNS提供了诸如缓存(caching)等机制来帮助root名称服务器减轻负载。当所查询的域名不在缓存中时，解析总是不得不从root名称服务器开始。 

##### 名称解析的完整过程：
解析器会询问本地名称服务器（即解析器向本地名称服务器发送了一个递归解析查询请求）， 本地名称服务器查询本地缓存，匹配最接近查询域名的名称服务器，然后向其发起查询请求（迭代查询请求）， 每个本查询的名称服务器都尽力将本地名称服务器指引到命名空间中（沿着倒置的数向下查询），返回最终答案或下一级区域的权威服务器的信息。最终，本地名称服务器会询问权威名称服务器，而后者将告知其答案。 在此期间，本地名称服务器将根据收到的每个响应（无论是指引还是答案），来更新相应名称服务器的**RTT值(往返时间， roundtrip time, RTT)**，而该RTT值又有助于决定以后的域名解析将询问哪一个名称服务器。 

##### 基于RTT时间，选择权威名称服务器的算法： 
> 在本地名称服务器发送迭代解析请求给远程名称服务器后，远程名称服务器将下一级的权威服务器列表（远程名称服务器所知道的所有的）发送给本地名称服务器，本地名称服务器如何选择向下一级哪一个权威名称服务器发送迭代查询请求呢？ 选择RTT时间最小的一个。 






#### 查询工具

##### dig 举例
- `dig math.stackexchange.com`
- `dig +short math.stackexchange.com`
- `dig @4.2.2.2 math.stackexchange.com`
- `dig +trace math.stackexchange.com`
- `dig ns com`
- `dig +short ns com`
- `dig facebook.github.io`
- `dig -x 192.30.252.153`
- `dig a github.com`
- `dig ns github.com`
- `dig mx github.com`
##### 其他DNS工具
1. host 命令。 host命令可以看作是dig命令的简化版本，返回当前请求域名的各种记录。  host 命令也可以用于逆向查询，即从IP地址查询域名，等同于 `dig -x <ip>`。
    - `host github.com`
    - `host facebook.github.com`
2. nslookup 命令。 nslookup命令用于交互式地查询域名记录。 
```
$ nslookup
> facebook.github.io
```
3. whois命令
whois 命令用来查看域名的注册情况。 
    - `whois github.com`

##### 参考资料
- 

