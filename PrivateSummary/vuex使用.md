Vuex是一个专为Vue.js应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

其实最简单理解为，在我们写Vue组件中，一个页面多个组件之间想要通信数据，那你可以使用Vuex

### - Vuex ---实现组件间通讯

思路：将通讯变量存储在vuex的state -> 组件通过state获取相关变量

步骤：
组件调用computed中的dispatch方法 -> 触发主入口中的actions方法  -> 触发主入口的mutations的方法(commit方法触发) -> 达到改变state状态

#####1、通过npm下载vue-x

``js
npm install vuex --save
``

详细文档查看:

[官方文档](https://vuex.vuejs.org/zh/installation.html)


#####2、打开服务器
```1js
npm install dev
```

##注意
vue中打开服务指令是：

```js
npm install serve
```

#####3、配置主入口store

```js
引入：
	import Vue from 'vue'	//var Vue = require('vue')
	import Vuex from 'vuex'	//var Vuex = require('vuex')
	Vue.use(Vuex)

const store = new Vuex.Store({
	state: {
			searchText: "",
			isShowGallery: false,
			galleryImg: ''
		},
		mutations: {
			setChangeText(state, data) {
				state.searchText = data;
			},
	//		setChangeGallery(state, data) {
	//			state.headPhoto = data;
	//		}
		},
		// 通过commit方法提交直接触发mutations
		actions: {
			setSearchText(context, data) {
				context.commit('setChangeText', data);
			},
	//		setShowGallery(context, data) {
	//			context.commit('setChangeGallery', data);
	//		}
		},
		// 只有在store中定义了getters方法，子组件才能配合computed计算属性动态获取state值
		getters: {
			getSearchText(state) {
				//处理数据
				return state.searchText
			},
	//		getShowGallery(state) {
	//			return state.headPhoto
	//		}
	}
})
```
子组件传递变量，存储到state(通过dispatch的方法)
```js
	methods:{
		setSearchText() {
			this.$store.dispatch("setSearchText", this.searchText);
		}	
	}
```


组件1获取state状态：
```js
	computed:{
		searchText() {
			return this.$store.getters.getSearchText;
			//或者return this.$store.state.searchName
		}
	}
```


####最快捷的方法直接获取state中的值
配置好主入口的相关变量后,在子组件中通过强势方法去获取
```js
computed:{
	getXXX(){
		return this.$store.state.XXXX;
	}
}
```
