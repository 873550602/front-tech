### 介绍
简单的说，koa-passport就是用于统一管理鉴权策略的一个工具，可通过一组可扩展的插件来完成相关鉴权任务
### 版本
4.x
### 安装
```javascript
npm install koa-passport
// or
yarn add koa-passport
```
### 基本使用

```javascript
// app.js
const app = require('koa')()
const passport = require('koa-passport')
const session = require('koa-session')

// 配置session
app.keys = ['this is secret'] //签名私钥
app.use(session({}, app))

// 调用两个初始化方法
app.use(passport.initialize({
    userProperty:string // 可以认为是存储策略的属性，比如 'user','task'
}))
app.use(passport.session())

```

### 配置策略
```javascript
const passport = require('koa-passport')
const LocalStrategy = require('passport-local').Strategy

const getUser= ()=> {
    return {
        account: 'zhangsan',
        password: '123456'
        ...
    }
}

passport.use(new LocalStrategy(
    {
        username: 'account',
        password: 'password'
    }, // 这里默认值是 username和password,如果前端字段不是默认字段，可以在此处修改
  function(username, password, done) {
    User.findOne({ username: username }, function (err, user) {
      // 自定义鉴权策略
      const user = getUser()
      if(user.account === username&&user.password===password) {
          do(null,user)
      } else {
          do(null,false)
      }
    });
  }
));
```

### 序列化与反序列化
passport为了保持持久的登录会话，经过身份认证的用户必须序列号到session,并在后续发出请求时反序列化化
```javascript
passport.serializeUser(function(user, done) {
  // 将id放入session
  done(null, user.id);
});

passport.deserializeUser(function(id, done) {
 // 通过id获取用户信息
  User.findById(id, function (err, user) {
    done(err, user);
  });
});
```

### 请求认证
```javascript
app.post('/login',(ctx)=>{
    ctx.passport.authenticate('local',(err,user)=> {
        if(user) {
            ctx.body = "success"
        } else {
            ctx.body = "error"
        }
    })
})
```


### API
```javascript
app.use(async ctx => {
  ctx.isAuthenticated() // 是否已登录
  ctx.isUnauthenticated() // 是否未登录
  await ctx.login() //登录
  ctx.logout() // 退出
  ctx.state.user //获取用户信息
})
```

