#rem布局

1、引入rem.js文件
```js
/* 
*  computed_rem 计算rem
*  @params picture_width: 设计图的宽度 number ( 750 )
*  @params rem_width    : 使用的rem宽度 number ( 100 ) 
*  return void
*/
const computed_rem = ( picture_width = 750, rem_width = 100 ) => {
  let html = document.documentElement
  html.style.fontSize = html.clientWidth / ( picture_width / ( 2 * rem_width ) ) + 'px'

  window.onresize = () => {
      let html = document.documentElement
      html.style.fontSize = html.clientWidth / ( picture_width / ( 2 * rem_width ) ) + 'px'    
  }
}

export default computed_rem
```

2、设置全局html的字体大小
```js
// 字体大小必须为固定值
font-size: 100px;
```