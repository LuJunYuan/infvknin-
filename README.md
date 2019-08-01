##请求文件
import Vue from 'vue'
import axios from 'axios'
// 设置统一路径
axios.defaults.baseURL = "http://127.0.0.1:8899/api/public/v1"

//拦截器
axios.interceptors.request.use(function (config) {
  if (wx.getStorageSync('token')) {
    config.headers.Authorization = wx.getStorageSync('token')
  }
  return config
})

// 适配器 替换到底层发送网络请求的方式
axios.defaults.adapter = function (config) {

  return new Promise((resolve, reject) => {
    // 发送请求前打开加载页面
    mpvue.showLoading({
      title: '拼命加载中.....',
      mask: true
    })
    // 发送请求
    mpvue.request({
      url: config.url, //开发者服务器接口地址",
      data: config.data, //请求的参数",
      method: config.method,
      header: config.headers, // 设置请求的 header
      dataType: 'json', //如果设为json，会尝试对返回的数据做一次 JSON.parse
      success: res => {
        resolve(res)
      },
      fail: err => {
        reject(err)
      },
      complete() {
        // 当页面加载完成后关闭
        mpvue.hideLoading()
      }
    })
  })
}

Vue.prototype.$axios = axios
#传值
	4、页面传值给组件
		页面  属性名=值
		组件 properties

	5、组件传值给页面
		组件 triggerEvent
		页面 注册自定义事件，写好函数来接收
		
#滚动 (获取滚出去的距离)
onPageScroll({scrollTop}){

}
#返回顶部
wx.pageScrollTo({
    scrollTop:0,  //返回顶部
    duration:300   //时间
})
# wx.getSystemInfoSync() 获取可使用窗口高度
#滚动内容<scroll-view  scroll-y></scroll-view >
#图片预览
 wx.previewImage({
         current,  //当前显示图片的链接
        //  map遍历this.goodsu.pics中的元素,返回item.pics_big
         urls:this.goodsu.pics.map(item => item.pics_big) //需要预览的图片链接列表,

#获取收货地址
wx.chooseAddress({
 success:res=>{
        保存在本地
 },
 fail:(({xxx})=>{

 })
})
#提示框(无选择按钮)
 wx.showToast({
    title:'请选择商品结算',
    icon:'none',
    duration:2000,
    mask:true,
    success:res=>{
    }
  });

#提示框(有选择按钮)
 wx.showModal({
              title: '提示', //提示的标题,
              content: '请去我的页面打开授权选择收货地址', //提示的内容,
              showCancel: false, //是否显示取消按钮,
              confirmText: '确定', //确定按钮的文字，默认为取消，最多 4 个字符,
              confirmColor: '#3CC51F', //确定按钮的文字颜色,
              success: res => {
                // confirm为 true 时，表示用户点击了确定按钮
                if (res.confirm) {
                  // console.log('aaaaaaaaaa');
                                   // 跳转的时候要加上/(跟目录)  
                  wx.switchTab({ url:'/pages/my/main'})
                }
              }
            })
#跳转到tabbar
  wx.switchTab({ url:'/pages/my/main'})  跳转的时候要加上/(跟目录)
 #navigatorTo 和 switchTab的选择
#页面跳转
如果在同一个页面栈中，我们跳转到页面用 navigateTo
 wx.navigateTo({ url: `/pages/pay/main?ids=${ids.join(',')}` })跳转的时候要加上/(跟目录)
#contact打开客服会话，如果用户在会话中点击消息卡片后返回小程序，可以从 bindcontact 回调中获得具体信息 
    <button class="contact-btn" open-type="contact"></button> 
#获取当前位置
  mpvue.getLocation({
 type: 'wgs84',
 success (res) {
   const latitude = res.latitude
   const longitude = res.longitude
   const speed = res.speed
   const accuracy = res.accuracy
 }
})

#  findIndex()this.goodsList.findIndex(item => item.goods_id === goods_id )
  findIndex() 方法返回传入一个测试条件（函数）符合条件的数组第一个元素位置。
findIndex() 方法为数组中的每个元素都调用一次函数执行：
# map数组的方法，他的内容就是他返回的内容  
  map() 方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。
map() 方法按照原始数组元素顺序依次处理元素。
#  some 遍历数组有返回值
   some() 方法用于检测数组中的元素是否满足指定条件（函数提供）。
  some() 方法会依次执行数组的每个元素：
##  Object.keys获取属性名出来

  ##     获取用户信息，可以从getuserinfo回调中获取到用户信息
        <button @getuserinfo="wxLogin" open-type="getUserInfo" class="wxLogin">登录后下单支付</button>
wxLogin(e){
  e.xxx(获取用户信息)
  success:async ({code})=>{
     获取code 结构{code}
  },
 
}
#block不是一个组件  
仅仅是用来包裹住代码，在条件渲染和列表渲染中用得比较多
#wx.getUserInfo 在没有授权之前，调用会报警告，
现在处理方式是在用户点击了按钮之后，再弹出授权框
#map:遍历旧数组，根据条件生成新的数组，新的数组的个数和旧数组一样

[1,2,3].map(item => item + 1) ===> [2,3,4]

#生命周期
	onLoad 页面加载
		onShow 页面显示
		onReady 页面初次渲染完成
		onHide 页面隐藏
		onUnload 页面卸载
#	事件处理（传参&冒泡&非冒泡）
		传参：自定义属性 data-*
		冒泡：bind事件名称
		阻止冒泡：catch
#	尺寸单位（适配） 
		rpx
		一般适用于宽高，但是字体一般还是用px
#template
    注意点：它不能单独显示，必须依附于Page
	可以把一些公共的内容，封装起来

步骤：
	1、定义
		创建两个文件 template.wxml、template.wxss
			template.wxml 最外层包裹一个 template并且要 写好name

	2、在页面中使用时候，先在页面的wxml和wxss中导入
		wxml <import />
		wxss @import "";

	3、在页面中通过 template 标签使用，并且通过data属性传值
		<template is="模板的名字" data="{{...对象}}" />
作用：
	显示一些简单的内容，他不具备交互能力，不能处理逻辑
#Component
注意点：它不能单独显示，必须依附于Page

步骤：
	1、创建好组件
		component.js  Component({})
		component.json "component": true

	2、在页面的json中注册
		usingComponents

	3、在页面中根据注册时候的名称来使用组件
		<注册组件时候的名称 />

	4、页面传值给组件
		页面  属性名=值
		组件 properties

	5、组件传值给页面
		组件 triggerEvent
		页面 注册自定义事件，写好函数来接收

#wxs ： 相当于小程序中的过滤器

#4、scroll-view设置样式的时候
		设置滚动的方向 scroll-x
		宽度不要写死，可以通过white-space:nowrap自动撑开
#weui-wxss 使用步骤
	1、从github克隆下来

	2、看懂代码是如何执行的
		见图

	3、导入weui-wxss到开发者工具下面去
		导入要导入dist目录
#跳转到列表页面
wx.navigatorTo 编程时  this.$router.push
navigator 声明式 router-link
#小程序中性能优化如何做?
setData
	错误的用法要避免
		1. 频繁的去 setData
			定时器的时间间隔要设置得合理
		2. 每次 setData 都传递大量新数据
			分页加载
		3. 后台态页面进行 setData
		4. 如果我们的模型数据和视图渲染没关系，不要使用setData

图片资源
	除了tabBar使用本地图片，其它通通通过网络加载
	长列表中，图片使用比较模糊的压缩图
	其它地方也不需要太过高清的图片

代码包大小
	及时清理没有使用到的代码和资源
#使用mpvue生成小程序的项目
vue init mpvue/mpvue-quickstart mp-pyg-31
根据提示一步一步往下走

切换到根目录，使用cnpm i 安装包

在根目录下执行 npm run dev
	在根目录下生成dist目录

打开开发者工具，选择根目录导入即可
#--------------
e.target & e.currentTarget
	e.target 事件源
	e.currentTarget 处理事件的那个节点

e.detail
	input中bindinput、bindblur、xxx
	组件传值给页面的时候

搜索
	看懂weui-wxss
	集成到自己的项目中
		list.wxss 导入 weui-searchbar.wxss
		list.wxml includes weui-searchbar.wxml
	根据关键字重新加载第一页的数据
  图片预览
	wx.previewImage
		urls 要预览的图片数组
		current 你当前要预览的那张图片
生成项目 & 运行
安装vue的脚手架

输入指令，生成项目
	vue init mpvue/mpvue-quickstart my-project

安装包
	
npm run dev
	生成dist目录

使用开发者工具导入并且预览

#授权API的使用和处理【重点】
	注意：
		大部分授权API，只会弹出一次，无论用户选择同意还是不同意

	用户同意的情况下，我们可以拿到数据做后续处理
		1、我们要显示在视图上
		2、存储到本地

	如果用户不同意，我们要给它一个提示，然后再后续处理
		弹出框 引导用户去某个页面中打开授权

加入购物车（注意数据存储的格式）
#slot
要点：
	1、slot是基于父子组件关系的，它是写在子组件中

	2、slot就是一个占位符，父组件在使用子组件的时候，可以给它传递任何显示的内容

	3、插槽非常灵活，它将控制权交给了使用它的组件

	4、插槽最厉害的地方在于，既要子组件给我们做事，同时控制权又要牢牢的把握在自己手里

	5、作用域插槽就是把每一行数据传给父组件
#小程序端登录的具体步骤：
	1、调用wx.login获取到临时的code

	2、获取到用户信息，通过button的open-type获取到注意：getUserInfo 这个接口即使拒绝了，也会再次弹出

	2、直接发送请求给自己的后台

	3、获取到token




















# mp-pyg-31

> A Mpvue project

## Build Setup

``` bash
# 初始化项目
vue init mpvue/mpvue-quickstart myproject
cd myproject

# 安装依赖
yarn

# 开发时构建
npm dev

# 打包构建
npm build

# 指定平台的开发时构建(微信、百度、头条、支付宝)
npm dev:wx
npm dev:swan
npm dev:tt
npm dev:my

# 指定平台的打包构建
npm build:wx
npm build:swan
npm build:tt
npm build:my

# 生成 bundle 分析报告
npm run build --report
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
