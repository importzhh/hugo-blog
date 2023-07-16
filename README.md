## 基于Hugo的静态页面博客

### 主题：
[hugo-theme-stack](https://github.com/CaiJimmy/hugo-theme-stack)


新建单Markdown文章
```shell
hugo new post/myfirst.md
```

新建带资源的Markdown文章
```shell
hugo new post/build/index.md 
```

预览展示草稿
```shell
hugo server -D
```
搭建教程教程参考：https://blog.tuwei.space

### 手动更新主题
Run:

```bash
hugo mod get -u github.com/CaiJimmy/hugo-theme-stack/v3
hugo mod tidy
```

> This starter template has been configured with `v3` version of theme. Due to the limitation of Go module, once the `v4` or up version of theme is released, you need to update the theme manually. (Modifying `config/module.toml` file)
> 此初学者模板已配置了 v3 主题版本。由于 Go 模块的限制，一旦发布 v4 主题或更高版本，就需要手动更新主题。（修改文件 config/module.toml ）