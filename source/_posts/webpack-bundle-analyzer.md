---
title: webpack-bundle-analyzer
date: 2020-08-07 15:27:17
tags:
- Javascript
- Vue
- Webpack
---

### Webpack bundle analyzer
- Project를 진행하고 있는데 Bundle의 사이즈가 커져 빌드시 워닝이 뜨고 있는 상태이다.
- 아무리 gzip 압축을 진행한다 한들 원채 사이즈가 크면 속도에 지대한 영향을 끼친다.
- Webpack bundle analyzer
   - npm install --save-dev webpack-bundle-analyzer
``` javascript
const path = require('path');
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer')
  .BundleAnalyzerPlugin;

module.exports = {
  publicPath: '/',
  devServer: {
    port: 8081,
    proxy: {
      '/api': {
        target: process.env.API_SERVER_URL,
        changeOrigin: true,
      },
    },
  },
  configureWebpack: {
    resolve: {
      alias: {
        '@': path.join(__dirname, 'src'),
        assets: path.resolve(__dirname, 'src', 'assets'),
      },
    },
    plugins: [new BundleAnalyzerPlugin()],
  },
  css: {
    loaderOptions: {
      sass: {
        prependData: `@import "@/assets/scss/main.scss";`,
      },
    },
  },
};
```