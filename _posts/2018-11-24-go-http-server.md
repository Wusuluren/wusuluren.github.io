---
layout: default-blog-md
title: 探究go源码之http server
category: markdown
---

golang标准库提供了很好用的http库，比如想要编写一个服务器程序，只需要简单几行代码：    
```go
http.HandleFunc("/", func(writer http.ResponseWriter, request *http.Request) {
    writer.Write([]byte("hello"))
})
http.ListenAndServe(":8000", nil)
```
下面深入标准库来看下go到底做了哪些事情。  
### 首先`http.HandleFunc`函数的定义如下：  
```go
func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
	DefaultServeMux.HandleFunc(pattern, handler)
}
```
这个函数里使用了`DefaultServeMux`这个标准看内置的路由结构变量，其定义如下：  
```go
type ServeMux struct {
	mu    sync.RWMutex
	m     map[string]muxEntry
	hosts bool // whether any patterns contain hostnames
}
```
这个结构使用了map来存储url路径与handler的映射关系，并使用mutex来加锁。

### 然后`http.ListenAndServe`的定义如下：
```go
func ListenAndServe(addr string, handler Handler) error {
	server := &Server{Addr: addr, Handler: handler}
	return server.ListenAndServe()
}
```
`Server`结构定义了服务器程序的参数，其中`Handler`成员变量负责处理url路由，默认是`http.DefaultServeMux`这个内置变量，如下所示：  
```go
Handler   Handler     // handler to invoke, http.DefaultServeMux if nil
```
而`Handler`是一种接口，定义如下：
```go
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}
```
所以，任何实现了`ServeHTTP`的对象都可以作为url路由，这种机制允许使用者灵活的替换标准库内置的路由结构，大大提高了标准库的灵活性和可扩展性。  

### 接下来就是服务端主要的处理流程
程序调用`ln, err := net.Listen("tcp", addr)`获取到一个tcp链接，然后就是一段服务端开发比较常见的代码模式了，主干代码如下：
```go
for {
		rw, e := l.Accept()
		c := srv.newConn(rw)
		go c.serve(ctx)
	}
```
在死循环里，accept新的链接，然后启动新的routine来处理这个链接。在新的routine里，调用`readRequest`来读取并解析http协议报文，然后调用`serverHandler{c.server}.ServeHTTP(w, w.req)`来处理请求。  
  
看到这里有没有眼熟，没错，这里调用了`ServeHTTP`，深入看下去会发现，其实最终会调用我们注册的handler。但我们做的只是把url路径和handler存储到map里，哪来的`ServeHTTP`方法？这就是go实现的比较巧妙的地方，上面提到了`DefaultServeMux.HandleFunc(pattern, handler)`函数，进一步看下去是这样的
```go
func (mux *ServeMux) HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
	mux.Handle(pattern, HandlerFunc(handler))
}
```
这里做了一次类型转换，把我们的handler转换成了`HandlerFunc`，而其定义如下： 
```go
type HandlerFunc func(ResponseWriter, *Request)

// ServeHTTP calls f(w, r).
func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request) {
	f(w, r)
}
```
看到这里就会发现，我们的handler的ServeHTTP方法就是自己。怎么样，函数也是变量，是对象，有点函数式编程的意思吧。

在代码里发现一个有趣的注释，http协议本来不能同时处理多个请求的，但`HTTP pipelining`技术可以做到这点，然而go并不打算实现这一技术，因为`HTTP pipelining`在现实世界里从来没有被实施过，而且更好的解决方案是http2。就像血统纯正的unix被后起之秀linux打败一样，这就是现实。