# GitHub Pages + Hexo搭建个人博客

## github 新建仓库

* 仓库名称必须为：<github用户名>.github.io
* 在仓库的setting里面的page可以查看github page相关信息

## hexo安装以及主题配置

### 安装启动流程

* 安装 Hexo

```
npm install -g hexo-cli
```

* 查看版本

```
hexo -v
```

* 创建一个项目 hexo-blog 并初始化

```
hexo init hexo-blog
cd hexo-blog
npm install
```

* 本地启动
```
hexo g
hexo server
```

### 主题更改

* 选择一个主题 git clone到themes 目录下，这里以fluid主题为例
* 下载主题包放到themes
* 指定主题

```
如下修改 Hexo 博客目录中的 _config.yml：

theme: fluid  # 指定主题
language: zh-CN  # 指定语言，会影响主题显示的语言，按需修改
```

* 创建「关于页」

首次使用主题的「关于页」需要手动创建：

```
hexo new page about
```

* 在hexo目录下建 _config.fluid.yml文档，copy fluid里面_config.yml，可以做到覆盖配置

```
hexo g -debug  查看配置修改是否成功
```

* 其他配置调整看官网

## 部署到github page
* 安装hexo-deployer-git
```
npm install hexo-deployer-git --save
```

修改根目录下的 _config.yml，配置 GitHub 相关信息
```
deploy:
  type: git
  repo: git@github.com:wuyu1001/wuyu1001.github.io.git
  branch: main
  token: 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
```
* Run hexo clean && hexo deploy.
* Check the webpage at username.github.io.