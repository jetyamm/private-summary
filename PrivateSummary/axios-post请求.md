#axios

##GET请求
```js
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

##POST请求
1、全局设置 or 局部设置
```js
import axios from 'axios';
import qs from 'querystring';
// url的前缀
window.axios = axios.create({
    baseURL: 'http://localhost:3001/',
});

// 请求拦截
window.axios.interceptors.request.use(request => {
    // console.log(request);
    request.data = qs.stringify(request.data);
    return request;
});
// 响应拦截
window.axios.interceptors.response.use(response => {
    // console.log(response);
    return response;
});
```
2、正常的POST请求
```js
 // 验证账号密码
window.axios.post('checkout', {
    username: username,
    password: pw
    })
    .then(function (response) {
    console.log(response);
    if(response == 'no'){
        window.confirm('输入的用户名或密码有误，请确认')     
    }else{
        // 正确跳转页面
        // react编程式导航
        this.props.history.push('/home/dashboard');
    }
    })
    .catch(function (error) {
    console.log(error);
    });
```