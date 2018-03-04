---
title: 【Webpack】webpack 4.x学习笔记
date: 2018-03-04 10:26:04
categories: Webpack
tags:
---

## [html-webpack-plugin](https://doc.webpack-china.org/guides/output-management/#设定-htmlwebpackplugin)

>如果我们更改了我们的一个入口起点的名称，甚至添加了一个新的名称，生成的包将被重命名在一个构建中，但是我们的index.html文件仍然会引用旧的名字。我们用 HtmlWebpackPlugin 来解决这个问题。

> [html-webpack-plugin](https://doc.webpack-china.org/plugins/html-webpack-plugin)简化了HTML文件的创建，以便为你的webpack包提供服务。这对于在文件名中包含每次会随着编译而发生变化哈希的 webpack bundle 尤其有用。 你可以让插件为你生成一个HTML文件，使用lodash模板提供你自己的模板，或使用你自己的[loader](https://doc.webpack-china.org/loaders)。

## [clean-webpack-plugin](https://doc.webpack-china.org/guides/output-management/#清理-dist-文件夹)

[clean-webpack-plugin](https://github.com/johnagan/clean-webpack-plugin) 会在每次构建前清理 /dist 文件夹

`new CleanWebpackPlugin(['dist'])`

## [webpack-dev-server](https://doc.webpack-china.org/guides/development/#使用-webpack-dev-server)

> [webpack-dev-server](https://github.com/webpack/webpack-dev-server) 提供了一个简单的 web 服务器，并且能够实时重新加载(live reloading)

## [uglifyjs-webpack-plugin](https://doc.webpack-china.org/guides/tree-shaking/#精简输出)

> 我们已经通过 import and export 语法，标识出了那些“未引用代码(dead code)”，但是我们仍然需要从 bundle 中删除它们。要做到这一点，我们将添加一个能够删除未引用代码(dead code)的压缩工具(minifier)——[uglifyjs-webpack-plugin](https://github.com/webpack-contrib/uglifyjs-webpack-plugin)

## [CommonsChunkPlugin](https://doc.webpack-china.org/guides/code-splitting/#防止重复-prevent-duplication-)

> [CommonsChunkPlugin](https://doc.webpack-china.org/plugins/commons-chunk-plugin) 插件可以将公共的依赖模块提取到已有的入口 chunk 中，或者提取到一个新生成的 chunk

## [ExtractTextPlugin](https://doc.webpack-china.org/plugins/extract-text-webpack-plugin)

> 用于将 CSS 从主应用程序中分离

## [bundle-loader](https://doc.webpack-china.org/loaders/bundle-loader)

> 用于分离代码和延迟加载生成的 bundle

## [promise-loader](https://github.com/gaearon/promise-loader)

> 类似于 bundle-loader ，但是使用的是 promises

## [动态导入](https://doc.webpack-china.org/guides/code-splitting/#动态导入-dynamic-imports-)

## [bundle 分析]()

>[webpack-chart](https://alexkuz.github.io/webpack-chart/): webpack 数据交互饼图。

> [webpack-visualizer](https://chrisbateman.github.io/webpack-visualizer/): 可视化并分析你的 bundle，检查哪些模块占用空间，哪些可能是重复使用的。
> 
> [webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer): 一款分析 bundle 内容的插件及 CLI 工具，以便捷的、交互式、可缩放的树状图形式展现给用户。

## [懒加载](https://doc.webpack-china.org/guides/lazy-loading/)

## [提取模板](https://doc.webpack-china.org/guides/caching/#提取模板-extracting-boilerplate-)

> [CommonsChunkPlugin](https://doc.webpack-china.org/plugins/commons-chunk-plugin): 删除了常用的 CommonsChunkPlugin。对于那些需要细粒度控制缓存策略的人，可以通过optimization.splitChunks和 optimization.runtimeChunk。 现在可以使用 module.rules[].resolve来配置解析。它与全局配置合并。
> 
> [配置 new webpack.optimize.CommonsChunkPlugin({ name: 'vendor' })这个之后,执行npm run build,出现报错.](https://www.imooc.com/qadetail/251317?t=404352)
> 
> [没有了CommonsChunkPlugin，咱拿什么来分包](http://blog.csdn.net/songluyi/article/details/79419118) 

## [模块标识符](https://doc.webpack-china.org/guides/caching/#模块标识符-module-identifiers-)

> vendor bundle 会随着自身的 module.id 的修改，而发生变化。

> [HashedModuleIdsPlugin](https://doc.webpack-china.org/plugins/hashed-module-ids-plugin): 该插件会根据模块的相对路径生成一个四位数的hash作为模块id, 建议用于生产环境。