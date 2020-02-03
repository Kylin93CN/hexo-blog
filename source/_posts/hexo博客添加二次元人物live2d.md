---
title: hexo博客添加二次元人物live2d
date: 2019-07-29 14:45:13
tags:
---

# 依赖
1. [hexo-helper-live2d](https://github.com/EYHN/hexo-helper-live2d)
2. [live2d-widget-models](https://github.com/xiazeyu/live2d-widget-models) ——2d模型

# 开始
1. 安装`hexo-helper-live2d`
```shell
npm install --save hexo-helper-live2d
```
 2. 选择并下载心仪的2d模型
 - `live2d-widget-model-chitose`
- `live2d-widget-model-epsilon2_1`
- `live2d-widget-model-gf`
更多见依赖中`2.live2d-widget-models`。
```shell
npm install --save live2d-widget-model-chitose
```
3. 添加配置

添加下面的配置到hexo的 **_config.yml** 文件中或者主题的 **_config.yml**

**model.use为使用的2d模型**
```yml
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  log: false
  model:
    use: live2d-widget-model-chitose
  display:
    position: right
    width: 150
    height: 300
  mobile:
    show: true
  react:
    opacity: 0.7
```

# 测试使用
执行代码`hexo clean` & `hexo server`，测试没问题执行`hexo g -d`发布。
