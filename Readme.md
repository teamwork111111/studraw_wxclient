```
画像系统后台接口文档地址
http://47.95.4.199:8081/api/swagger-ui.html
http://120.24.74.223:8081/swagger-ui.html

```
 
## 开始使用
下载源码解压，复制根目录的 `/colorui` 文件夹到你的根目录

`App.vue` 引入关键Css `main.css` `icon.css`
```
<style>
    @import "colorui/main.css";
	@import "colorui/icon.css";
	@import "app.css"; /* 你的项目css */
	....
</style>
```

------

## 使用自定义导航栏
导航栏作为常用组件有做简单封装，当然你也可以直接复制代码结构自己修改，达到个性化目的。

`App.vue` 获得系统信息
```
onLaunch: function() {
	uni.getSystemInfo({
		success: function(e) {
			// #ifndef MP
			Vue.prototype.StatusBar = e.statusBarHeight;
			if (e.platform == 'android') {
				Vue.prototype.CustomBar = e.statusBarHeight + 50;
			} else {
				Vue.prototype.CustomBar = e.statusBarHeight + 45;
			};
			// #endif
			// #ifdef MP-WEIXIN
			Vue.prototype.StatusBar = e.statusBarHeight;
			let custom = wx.getMenuButtonBoundingClientRect();
			Vue.prototype.Custom = custom;
			Vue.prototype.CustomBar = custom.bottom + custom.top - e.statusBarHeight;
			// #endif		
			// #ifdef MP-ALIPAY
			Vue.prototype.StatusBar = e.statusBarHeight;
			Vue.prototype.CustomBar = e.statusBarHeight + e.titleBarHeight;
			// #endif
		}
	})
},
```

`pages.json` 配置取消系统导航栏
```
"globalStyle": {
	"navigationStyle": "custom"
},
```
复制代码结构可以直接使用，注意全局变量的获取。

使用封装,在`main.js` 引入 `cu-custom` 组件。
```
import cuCustom from './colorui/components/cu-custom.vue'
Vue.component('cu-custom',cuCustom)
```

`page.vue` 页面可以直接调用了
```
<cu-custom bgColor="bg-gradual-blue" :isBack="true">
	<block slot="backText">返回</block>
	<block slot="content">导航栏</block>
</cu-custom>
```
| 参数       | 作用   |类型    |  默认值 |
| --------   | -----:  |-----:  | :----:  |
| bgColor    | 背景颜色类名 |String  |   ''    |
| isBack     | 是否开启返回 | Boolean |   false |
| bgImage    | 背景图片路径 | String  |  ''     |

| slot块       | 作用   |
| --------   | -----:  |
| backText    | 返回时的文字 | 
| content     | 中间区域 | 
| right    | 右侧区域(小程序端可使用范围很窄！)  | 


------


## 使用自定义Tabbar
这部分暂时没有封装，思路可以参考下我的源码，原理是一个主页面引入多个页面，在主页面进行切换显示。这样可以解决切换时闪烁的问题。


------


## 更新日志

 * 2019年4月25日 v2.1.6
    *  删除var变量 向下兼容安卓APP
	*  优化单选等表单控件

 * 2019年4月25日 v2.1.5
    *  优化图片上传
    *  优化一些点击区域过小
    *  优化图标旋转
    *  优化demo显示
    *  优化阴影
    *  修复支付宝小程序编译出错

 * 2019年4月14日 v2.1.4
    *  新增多种阴影
	*  修复一些var属性的错误
	*  修复轮播图控制点隐藏不了
	*  修改图标类名
	*  修复表单组件里上传图片 ios没有图片显示问题

 
 * 2019年4月01日 v2.1.3
    *  优化代码,支持支付宝小程序
	*  textarea 样式还原

 * 2019年3月28日 v2.1.2
	*  修复列表组件样式
	*  优化主样式代码

 * 2019年3月27日 v2.1.1
    *  新增多种扩展
    *  优化堆叠轮播图
    *  优化消息列表
	*  优化导航栏的封装
	*  修复卡片评论错位(3月27日16:32:17)

* 2019年3月25日 v2.1.0
    *  完成元素，组件移植
	*  icon文件更改名称，避免图标冲突
	*  针对不同端口做了优化