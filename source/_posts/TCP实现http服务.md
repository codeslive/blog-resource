---
title: TCP实现http服务
tages: [网络]
categories: 网络
description: TCP实现http服务
comments: true
cover:  https://static-youdao-note.oss-cn-shenzhen.aliyuncs.com/images/202306051707718.webp?x-oss-process=style/webp
---

```typescript
// Description: TCP 服务端
import net from 'net';
// html 模板
const html: string = `<h2>xiaokang zs</h2>`;
// 响应头
const headers = [
  'HTTP/1.1 200 OK',
  'Content-Type: text/html;charset=utf-8',
  'Content-Length: ' + html.length,
  'Date: ' + new Date(),
  `\r\n`,
  html
]
// 创建服务
const server = net.createServer((socket) => {
  socket.on('data', (data) => {
    if (/GET/.test(data.toString())) {
      socket.write(headers.join('\r\n'));
      socket.end();
    }
  });
});
// 监听端口
server.listen(3000, () => {
  console.log('server start at 3000');
});
```

