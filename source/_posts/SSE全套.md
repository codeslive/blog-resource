---
title: SSE
tages: [网络]
categories: 网络
description: SSE
comments: true
cover:  https://static-youdao-note.oss-cn-shenzhen.aliyuncs.com/images/202306051213334.webp?x-oss-process=style/webp
---



> SSE（Server-Sent Events）是一种用于服务器向客户端推送实时数据的技术。它基于HTTP协议，允许服务器实时地向客户端发送数据，而无需客户端请求。这使得SSE成为一种非常适合实时数据推送的解决方案。
>
> SSE可以用于很多场景，例如：
>
> 1. 实时通知：SSE可以用于向客户端推送实时通知，例如新的消息、新的订单等等。
>
> 2. 实时数据更新：SSE可以用于向客户端推送实时数据更新，例如股票行情、天气预报等等。
>
> 3. 实时聊天：SSE可以用于实现实时聊天功能，例如在线客服、聊天室等等。
>
> 总之，SSE是一种非常强大的实时数据推送技术，可以应用于很多实时场景，提升用户体验和应用性能。

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SSE</title>
</head>
<style>
  .content {
    border: 1px solid gainsboro;
    /* text-align: center; */
    width: 50%;
    height: 300px;
    margin: 20px auto;
    overflow: hidden;
    border-radius: 5px;
    line-height: 30px;
    /* 溢出换行 */
    word-wrap: break-word;
    /* 溢出省略号 */
    text-overflow: ellipsis;
    /* 设置渐变 */
    background: deepskyblue;
    color: white;
  }

  .content p{
    text-align: center;
    font-size: 20px;
    font-weight: bold;
  }

  .content .txt{
    width: 90%;
    margin: 0 auto;
  }

  
</style>

<body>
  <div class="content">
    <p>回车输出内容</p>
    <div class="txt"><span id="message"></span></div>
  </div>

  <script>
    document.addEventListener('keydown', (e) => {
      if (e.keyCode == 13) {
        const sse = new EventSource('http://localhost:3000/api/sse');
        sse.addEventListener('message', (e) => {
          console.log(e.data);
          const message = document.querySelector('#message');
          message.innerHTML += e.data;
        })
      }
    })
  </script>
</body>

</html>
```

```typescript
import express from "express";
import fs from "fs";
const app = express();

app.get('/api/sse', (req, res) => {
  // 允许跨域
  res.setHeader('Access-Control-Allow-Origin', '*');
  // 设置sse请求头
  res.writeHead(200, {
    'Content-Type': 'text/event-stream', // 核心代码设置对应的请求头信息
  });

  // 读取文件内容
  const txt = fs.readFileSync('./index.txt', 'utf-8');
  // 将文件内容转换为数组
  const arr = txt.split('');
  let current = 0;
  // 每隔300ms发送一次数据
  let timer = setInterval(() => {
    if (current < arr.length) {
      res.write(`data: ${arr[current]}\n\n`);
      current++;
    } else {
      clearInterval(timer);
    }
  }, 100)
})

app.listen(3000, () => {
  console.log('server is running at http://localhost:3000');
});
```

