# Vue2.0学习笔记

>1. 环境区分

```js
//util.js
export default {
  COOKIE_NAME_TOKEN : 'fp_tk',
  LOCAL_KEY_LOGIN:'loginObject',
  getApiBaseUrl(){
    var env = process.env.NODE_ENV;
    if(env == 'development'){
      return 'http://localhost:8080/';
    }else{
      return 'https://www.xxx.com/';
    }
  }
}
```
引入的时候：
```js
import  util from '../util/util.js'
```
> 2. vue-router传参
```js
//from页面
this.$router.push({
        name: 'add_item',
        params: item,
      })
//to页面
mounted(){
    var params = this.$route.params;
}
//如果to页面存在keep-alive页面缓存，mounted不会调用，则：
activated(){
    var params = this.$route.params;//从明细返回的
}
//如果不想使用to页面的缓存
beforeRouteLeave(to, from, next){
    this.$destroy();
    next();
}
```
> 注意：from页面是**router**，to页面是**route**，少了一个**r**。
