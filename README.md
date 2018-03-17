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
