#Redux
概念：用户在view层通过 store.dispatch 来发送 action ，当 store 接收到用户传递过来的 action 后，会把 state 和 action 传给 reducer ，而 reducer 会根据 action 的 type，来返回一个新的 state。而当 store 有变化的时候，store 就会调用监听函数 store.subscribe ，从而使得view层发生变化。

###注意：
（1）react-redux提供两个方法：
* connect  
* Provider
（2）redux和vux都是页面刷新后仓库数据重新初始化，既重置，例如登录的用户信息不能用两者来存储，只能用sessionstorage或者localstorage来存储
-------------------
1、安装模块
```js
npm install react-redux --save
npm install redux --save
```

2、初始化仓库reducer.js
```js
let inintStore = {
    student: [],
    simpleStudent: {}
} 

const reducer = function (state = inintStore, action) {
        //指定state修改逻辑
        //根据不同的action操作旧的stateconst reducer= function(state-cartPage, action){
        switch (action.type) {
            //添加
            case 'STU_ADD' :
                return {
                ...state,
                simpleStudent: action.payload.stu
            }
            // 不能缺少default，否则会遇到undefined的坑
            default: return state;
        }
    }

export default reducer;
```

3、在App.js引入模块方法
```js
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import reducers from './reducers'

const store = createStore(reducers)

// 可以通过subscribe方法来监听数据的变化
store.subscribe(()=>{
  console.log('store',store.getState());
});

ReactDOM.render(
  // Provider包住所有路由并传store过去
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

4、Badd.js连接组件并且指定暴露的数据（使用connect方法）

* dispatch函数相当于action，仓库state中的action.payload.stu值就相当于dispatch中需要修改的值，仓库中的值或者方法可以通过this.props来获取仓库对象
```js
// 相当于Vue的getters函数
let mapStateToProps = function(state){
    return {
        student: state.simpleStudent
    }
}
// 相当于Vue的dispatch触发Mutation修改函数
let mapDispatchToProps = function(dispatch){
    return {
        addToStu:(stu)=>{
            dispatch({
                type: 'STU_ADD',
                payload:stu
            })
        }
    }
}

// 连接组件并且指定暴露的数据
EditableTable = connect(mapStateToProps,mapDispatchToProps)(EditableTable);
```

5、通过this.props获取仓库对象
```js
// 修改仓库值
this.props.addToStu(stu);
// 获取仓库值
this.props.student；
```

[redux工作原理图](https://img-blog.csdn.net/20180424095926281?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NTM0ODIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


##参考文档
[redux中文文档](https://www.redux.org.cn/)
[React——react-redux](https://blog.csdn.net/qq_35534823/article/details/80062915)