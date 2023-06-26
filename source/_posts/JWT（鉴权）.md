---
title: JWT（鉴权）
tages: [网络]
categories: 网络
description: JWT（鉴权）
comments: true
cover:  https://static-youdao-note.oss-cn-shenzhen.aliyuncs.com/images/202306261848915.webp?x-oss-process=style/webp
---

#  演示案例

### 一、index.ts

```typescript
// 导入 express
const express = require('express');
// 导入 cors
const cors = require('cors');
// 导入 jsonwebtoken commonjs规范
const jwt = require('jsonwebtoken');
// 导入Request Response 类型
import { Request, Response } from 'express';

const app = express();
app.use(cors());
app.use(express.json());
app.use(express.urlencoded({ extended: false }));

let Key = 'XKZS';

const user = {
  id: 1,
  username: 'admin',
  password: '123456'
}

// 登录返回前端 token 用于授权
app.post('/api/login', (req: Request, res: Response) => {
  // 如果账号密码错误, 返回
  if (req.body.username !== user.username && req.body.password !== user.password) {
    return res.status(403).json({
      message: '账号或密码错误'
    });
  }

  // 生成 token
  res.json({
    message: '登录成功',
    token: jwt.sign({ id: user.id }, Key, { expiresIn: '1h' }),
    code: 200,
  })

});

// 列表接口但是必须是授权状态才能访问 否则返回 403
app.get('/api/list', (req: Request, res: Response) => {
  // 获取前端传递过来的 token
  const token = req.headers.authorization as string; // 前端会把 token 放在请求头中 authorization 字段中
  jwt.verify(token, Key, (err: any, decode: any) => {
    if (err) {
      return res.status(403).json({
        message: '无权限访问'
      });
    }
    res.json({
      message: 'ok',
      list: [
        { id: 1, name: '张三' },
        { id: 2, name: '李四' },
        { id: 3, name: '王五' },
      ]
    })
  })

});

// 监听端口
app.listen(9000, () => {
  console.log('服务器启动成功, 地址是: http://localhost:9000');
});



```

### 二、index.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>index</title>
</head>

<body>
  <!-- 登录表单 -->
  <form>
    <input type="text" id="username" placeholder="请输入用户名">
    <input type="password" id="password" placeholder="请输入密码">
    <input id="btn" type="button" value="登录">
  </form>

  <script>
    const btn = document.querySelector('#btn');
    const username = document.querySelector('#username');
    const password = document.querySelector('#password');

    // 登录函数
    function login (url) {

      // 判断用户名和密码是否为空
      if(username.value === '' || password.value === '') {
        alert('用户名或密码不能为空');
        return;
      }

      fetch(url, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          username: username.value,
          password: password.value
        })
      }).then(res => res.json()).then(res => {
        if (res.code !== 200) {
          alert(res.message);
          return;
        }      
        // 保存token
        localStorage.setItem('token', res.token);
        // 跳转到列表页
        location.href = './list.html';
      })
    }

    // 绑定点击事件
    btn.addEventListener('click', () => {
      login('http://localhost:9000/api/login');
    });


  </script>
</body>

</html>
```



### 三、list.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>list</title>
</head>

<body>
  <script>
    // 封装请求函数
    function getList (url) {
      // 获取token
      const token = localStorage.getItem('token');
      fetch(url, {
        headers: {
          'Authorization': token
        }
      }).then(res => res.json()).then(res => {
        console.log(res);
      })
    }
    // 调用请求函数
    getList('http://localhost:9000/api/list');
  </script>
</body>

</html>
```

