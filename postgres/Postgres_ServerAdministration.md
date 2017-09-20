
 
### PostgreSQL 允许远程访问设置方法  postGres create User

 安 装PostgreSQL数据库之后，默认是只接受本地访问连接。如果想在其他主机上访问PostgreSQL数据库服务器，就需要进行相应的配置。

配置远 程连接PostgreSQL数据库的步骤很简单，只需要修改data目录下的pg_hba.conf和postgresql.conf。

#### pg_hba.conf：配置对数据库的访问权限，

#### postgresql.conf：配置PostgreSQL数据库服务器的相应的参数。

下面 介绍配置的步骤：

 1.修改pg_hba.conf文件，配置用户的访问权限（#开头的行是注释内容）：
```
    # TYPE DATABASE  USER    CIDR-ADDRESS     METHOD
    # "local" is for Unix domain socket connections only
    local all    all               trust
    # IPv4 local connections:
    host  all    all    127.0.0.1/32     trust
    host  all    all    192.168.1.0/24    md5
    # IPv6 local connections:
    host  all    all    ::1/128       trust
```
其中，第7条是新添加的内容，表示允许网段192.168.1.0上的所有主机使用所有合法的数据库用户名访问数据库，并提供加密的密码验证。

其中，数字24是子网掩码，表示允许192.168.1.0--192.168.1.255的计算机访问！


以上是网上检测结果，没有检验其正确性。不过一般的更改，不需要特别严格的权限控制时，可以直接将第7行，IPv4中换成

### host    all             all             0.0.0.0/0              trust         即可。

 

2.修改postgresql.conf文件，将数据库服务器的监听模式修改为监听所有主机发出的连接请求。

定位到#listen_addresses=’localhost’。PostgreSQL安装完成后，默认是只接受来在本机localhost的连接请 求。

### 将行开头都#去掉，将行内容修改为listen_addresses=’*'来允许数据库服务器监听来自任何主机的连接请求
 
