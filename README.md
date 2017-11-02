# blog系统操作

## 写作流指令
1. 创建新文章 `hexo new "my title"`
2. 本地查看文章 `hexo server`
3. 生成静态文件及部署 `hexo g -d` 或 `hexo d -g`


## 指令（部分）https://hexo.io/zh-cn/docs/commands.html
- 初始化网站
`hexo init [folder]`

- 创建新文章
`hexo new [layout] <title>`

- 生成静态文件
`hexo generate` === `hexo g`

- 生成静态文件后立即部署网站
` hexo g -d`

- 启动本地服务器,http://localhost:4000/
`hexo server`

- 部署网站
`hexo deploy` === `hexo d`

- 部署之前先生成静态文件
`hexo d -g`

- 清除缓存文件和已生成的静态文件
`hexo clean`

- 查看hexo版本
`hexo version`