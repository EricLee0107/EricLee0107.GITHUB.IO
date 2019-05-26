---
title: leaf框架源码-network模块
author: 李志伟
tags: [Golang,leaf]
copyright: true
categories:
  - Golang源码
toc: true
date: 2018-07-25 15:57:50
---


摘要：leaf服务器通过tcp连接并收取消息。

<!--more-->

#### 结构

```
// tcp_server.go

type TCPServer struct {
	Addr            string      // 服务器地址:端口
	MaxConnNum      int         // 最大连接数
	PendingWriteNum int         // 写通道容量
	NewAgent        func(*TCPConn) Agent
	ln              net.Listener
	conns           ConnSet
	mutexConns      sync.Mutex
	wgLn            sync.WaitGroup
	wgConns         sync.WaitGroup

	// msg parser
	LenMsgLen    int            // 消息长度标识所占的字节长度
	MinMsgLen    uint32         // 最小消息长度
	MaxMsgLen    uint32         // 最大消息长度
	LittleEndian bool           // 大小端设置（默认为false)
	msgParser    *MsgParser
}
```

```
// tcp_msg.go
// --------------
// | len | data |
// --------------
type MsgParser struct {
	lenMsgLen    int
	minMsgLen    uint32
	maxMsgLen    uint32
	littleEndian bool
}
```

在路由器设置时如果config中配置了TCPAddr则会调用tcp_server.go的Start函数来启动TCP服务。

#### TCP连接

TCP启动通过gate模块调用TCPServer的Start方法来实现。

**TCPserver的Start方法**

```
// tcp_server.go
func (server *TCPServer) Start() {
	server.init()
	go server.run()
}
```

Start方法主要包含两部分，init方法和run方法。

**init方法**

init方法主要包含三个部分：监听ip地址的指定端口、检查设置合法性、设置TCPServer的一些参数。

监听地址
```
// tcp_server.go
    ln, err := net.Listen("tcp", server.Addr)
	if err != nil {
		log.Fatal("%v", err)
	}
```

检查最大连接数、‘写通道’容量及Agent

```
// tcp_server.go
	if server.MaxConnNum <= 0 {
		server.MaxConnNum = 100
		log.Release("invalid MaxConnNum, reset to %v", server.MaxConnNum)
	}
	if server.PendingWriteNum <= 0 {
		server.PendingWriteNum = 100
		log.Release("invalid PendingWriteNum, reset to %v", server.PendingWriteNum)
	}
	if server.NewAgent == nil {
		log.Fatal("NewAgent must not be nil")
	}
```

对TCPServer进行一些参数初始化设置

```
// tcp_server.go
    server.ln = ln
	server.conns = make(ConnSet)

	// msg parser
	msgParser := NewMsgParser()
	msgParser.SetMsgLen(server.LenMsgLen, server.MinMsgLen, server.MaxMsgLen)
	msgParser.SetByteOrder(server.LittleEndian)
	server.msgParser = msgParser
```

**run方法**

在Start方法中是在一个新的协程中调用的run方法。

run方法主要是通过for循环无限循环接收新的连接，并对每个连接启动新的协程进行处理。

for循环内部内容：

1. 接收连接请求

```
// tcp_server.go
conn, err := server.ln.Accept()
```

2. 检查连接数是否超过最大连接数，并将连接存入TCPServer的conns字段。

```
// tcp_server.go
        server.mutexConns.Lock()
        if len(server.conns) >= server.MaxConnNum {
        	server.mutexConns.Unlock()
        	conn.Close()
        	log.Debug("too many connections")
        	continue
        	}
        	server.conns[conn] = struct{}{}
        	server.mutexConns.Unlock()
```

3. 创建Agent

```
// tcp_server.go
    tcpConn := newTCPConn(conn, server.PendingWriteNum, server.msgParser)//创建Tcp连接
    agent := server.NewAgent(tcpConn)
```


4. 协程中执行agent.Run()

```
// tcp_server.go
go func() {
			agent.Run()

			// cleanup
			tcpConn.Close()
			server.mutexConns.Lock()
			delete(server.conns, conn)
			server.mutexConns.Unlock()
			agent.OnClose()

			server.wgConns.Done()
		}()

```

**开始接收消息**

agent的Run方法，其中agent结构为：

```
// gate.go
type agent struct {
	conn     network.Conn
	gate     *Gate
	userData interface{}
}
```

agent的Run()方法
```
func (a *agent) Run() {
	for {
		data, err := a.conn.ReadMsg()
		if err != nil {
			log.Debug("read message: %v", err)
			break
		}
		if a.gate.Processor != nil {
			msg, err := a.gate.Processor.Unmarshal(data)
			if err != nil {
				log.Debug("unmarshal messagefff error: %v", err)
				break
			}
			err = a.gate.Processor.Route(msg, a)
			if err != nil {
				log.Debug("route messagefff error: %v", err)
				break
			}
		}
	}
}
```

1. 循环接收消息

```
		data, err := a.conn.ReadMsg()
```
ReadMsg方法,因为agent的conn是在上面创建Agent时通过`newTCPConn`函数创建的，所以结构为`*TCPConn`,所以调用的ReadMsg方法为TCPConn的ReadMsg方法。

```
// tcp_conn.go
func (tcpConn *TCPConn) ReadMsg() ([]byte, error) {
	return tcpConn.msgParser.Read(tcpConn)
}
```

*MsgParser的Read方法`func (p *MsgParser) Read(conn *TCPConn) ([]byte, error)`

获取消息长度的byte值
```
// tcp_msg.go
	var b [4]byte
	bufMsgLen := b[:p.lenMsgLen]

	// read len
	if _, err := io.ReadFull(conn, bufMsgLen); err != nil {
		return nil, err
	}
```

*ReadFull从conn精确地读取len(bufMsgLen)字节数据填充进bufMsgLen。函数返回
写入的字节数和错误（如果没有读取足够的字节）。只有没有读取到字节时才可能
返回EOF；如果读取了有但不够的字节时遇到了EOF，函数会返回ErrUnexpectedEOF。
只有返回值err为nil时，返回值n才会等于len(bufMsgLen)。*

根据大小端解析bufMsgLen为int值
```
// tcp_msg.go
	switch p.lenMsgLen {
	case 1:
		msgLen = uint32(bufMsgLen[0])
	case 2:
		if p.littleEndian {
			msgLen = uint32(binary.LittleEndian.Uint16(bufMsgLen))
		} else {
			msgLen = uint32(binary.BigEndian.Uint16(bufMsgLen))
		}
	case 4:
		if p.littleEndian {
			msgLen = binary.LittleEndian.Uint32(bufMsgLen)
		} else {
			msgLen = binary.BigEndian.Uint32(bufMsgLen)
		}
	}
```

这里需要对`littleEndian`这个字段做一些说明：
有几个结构中有`littleEndian`这个字段。
- `type Gate struct`
- `type TCPServer struct`
- `type MsgParser struct`
这几个结构中的`littleEendian`，Gate赋值给TCPServer,TCPServer赋值给MsgParser，所以根本上来自于Gate（Gate读取的conf.go中的配置）。

另外在使用protobuf时，protobuf.go中的`type Processor struct`结构中也存在`littleEndian`，这个实在调用protobuf中的`NewProcessor`
函数时赋值的默认为false，但这个`littleEndian`只是负责protobuf解析时是否使用小端序。所以如果在使用protobuf且使用小端序时需要设置
Processor中的littleEndian为true（Processor提供了SetByteOrder方法）,且conf.go中需要设置LittleEndian为true。

判断消息长度合法性
```
// tcp_msg.go
	// check len
	if msgLen > p.maxMsgLen {
		return nil, errors.New("message too long")
	} else if msgLen < p.minMsgLen {
		return nil, errors.New("message too short")
	}
```

读取消息数据
```
// tcp_msg.go
	// data
	msgData := make([]byte, msgLen)
	if _, err := io.ReadFull(conn, msgData); err != nil {
		return nil, err
	}
```

到此数据的得去就完成了，然后是数据的解析。

转[leaf框架源码-network模块(二)](https://llizhiwei.net/leaf-scode-2.html)




