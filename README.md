# Vue2.0学习笔记

## 1. 环境区分

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
## 2. vue-router传参
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

## 3.axios拦截请求与响应
```js
//设置baseUrl
Vue.axios.defaults.baseURL = config.getApiBaseUrl();
Vue.axios.interceptors.request.use((config)=>{
  Vue.$vux.loading.show({text: '加载中...'})//发请求之前展示loading
  return config;
}, (error) => {
  Vue.$vux.loading.hide()
  return Promise.reject(error);
});
Vue.axios.interceptors.response.use((response)=> {
  Vue.$vux.loading.hide();
  if(response.data.code == 0){//拦截响应，只有code是0才是正常
    return response;
  }else if(response.data.code == '500304'){//session失效，直接跳转到登录
    //清除本地数据
    util.clearLocal();
    //跳转到登录
    window.location.href = "/";
  }else{//code不是0，弹出异常信息，返回失败
    Vue.$vux.toast.text(response.data.message || '服务端异常，请稍后再试', 'middle');
    return Promise.reject({_errorMessage_:response.data.message,_errorCode_:response.data.code});
  }
},  (error)=> {
  Vue.$vux.loading.hide();
  Vue.$vux.toast.text("服务端异常，请稍后再试", 'middle');
  return Promise.reject(error);
});
```
