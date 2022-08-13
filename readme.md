##### 一个vue验证码插件，可验证九宫格定位，图形识别，语义识别，颜色识别以及用户鼠标操作行为

### 用例
```bash
# 安装依赖
npm i vue-grid-verify
```
```html
<!--验证模式，默认position，可选shape,使用图形验证-->
<vueGridVerify @result="getResult" mode="shape" />
```

```js
import vueGridVerify from 'vue-grid-verify'

methods:{
  getResult(result){
    console.log(result)
    // 返回验证结果
  }
}
```