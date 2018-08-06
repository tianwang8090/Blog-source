---
title: '使用vue-router的history模式需要的配置'
icon: fa-vuejs
date: 2018-08-06 09:36:46
categories:
tags:
---
# 使用vue-router的history模式需要的配置

## 1.环境配置
```js
/* dev.env.js */
module.exports = merge(prodEnv, {
  NODE_ENV: '"development"',
  APP_ROOT: '"/operation-decision-frontend/"',
  API_ROOT: '"//172.16.100.110:8081/operation-decision-maker/"'
})

/* prod.env.js */
module.exports = {
  NODE_ENV: '"production"',
  APP_ROOT: '"/operation-decision-frontend/"',
  API_ROOT: '"//172.16.100.110:8081/operation-decision-maker/"'
}
```

## 2. vue-router配置
```js
/* router.js */
const router = new Router({
  routes,
  mode: "history",
  base: process.env.APP_ROOT
});
```

## 3.项目打包配置
```js
/*  */
module.exports = {
  build: {
    env: require('./prod.env'),
    index: path.resolve(__dirname, '../dist/index.html'),
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    ...
  },
  dev: {...}
}
```

## 4. NGINX配置
```conf
server {
    listen       80;
    server_name  localhost;
    
    root E:/WWW/operation_decision/dist/;

    location /operation-decision-frontend {
        index index.html index.htm;
        try_files $uri $uri/ /index.html;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }
}
```
