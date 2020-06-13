## yarn/npm 源地址查看与修改



### npm，yarn查看源地址和换源地址

```javascript
npm config get registry  // 查看npm当前镜像源

npm config set registry https://registry.npmjs.org

yarn config get registry  // 查看yarn当前镜像源

yarn config set registry https://registry.yarnpkg.com  
```



### 镜像源地址部分如下：

```JavaScript
npm --- https://registry.npmjs.org/

npm --- https://registry.npm.taobao.org/

yarn --- https://registry.yarnpkg.com/

yarn --- https://registry.npm.taobao.org/

cnpm --- https://r.cnpmjs.org/

taobao --- https://registry.npm.taobao.org/

nj --- https://registry.nodejitsu.com/

rednpm --- https://registry.mirror.cqupt.edu.cn/

npmMirror --- https://skimdb.npmjs.com/registry/

deunpm --- http://registry.enpmjs.org/
```

