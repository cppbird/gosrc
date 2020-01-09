# flow control

## tcp

tcp 流控多用于传输数据包时协调发送/接受传输能力的机制，是end-to-end的。

## http 2.0

>[rfc7540](https://tools.ietf.org/html/rfc7540)

- [http2.0流控](https://tools.ietf.org/id/draft-montenegro-httpbis-http2-fc-principles-01.html) 用于应用层，是hop-to-hope的。
- 由于http 2.0是多路复用的，可以对于每一个steam都可以做流量控制；
- http2.0 流量控制的事定向的，由receiver声明，并且由sender遵守；
- 流控receiver可以选择关闭流量控制。
- receiver和sender对于每个stream约定一个window size，receiver通过传递窗口大小更新消息来和sender传输同步；
- 规范没有规定具体流量控制或者窗口大小更新的算法，需要自己实现。

## h2c/grpc

## 参考文献

- [tcp flow control](https://www.brianstorti.com/tcp-flow-control/)
- [http2 flow control](https://medium.com/coderscorner/http-2-flow-control-77e54f7fd518)
- [rfc 7540笔记](https://laike9m.com/blog/rfc7540-bi-ji-wu-flow-control,106/)
- [rfc7540](https://tools.ietf.org/html/rfc7540)
- [rfc7540中文翻译](https://quafoo.gitbooks.io/http2-rfc7540-zh-cn-en/content/chapter1/)