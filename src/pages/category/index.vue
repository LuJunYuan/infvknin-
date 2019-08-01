<template>
  <div class="categories">
    <!-- 左边 -->
<div>                                                        
      <!-- :style="{height:windowHeight+'px;'}"设置高度               scroll-y 滚动 -->
     <scroll-view :style="{height:windowHeight+'px;'}" class="categories-left" scroll-y >
       <!-- :style="{height:windowHeight+'px;'}" -->
       <div @click="catdian(index1)" :class="['categories-left-item',indexasd === index1 ? 'categories-left-active':'']" v-for="(item,index1) in categories" :key="item.cat_id">
        <text>{{item.cat_name }}</text>
        </div>
       
 </scroll-view >
  </div>
  <!-- 右边 -->
 <div>
            <!-- 点击按钮的时候传递了下标,显示下标的内容 -->
    <scroll-view :style="{height:windowHeight+'px;'}" class="categories-right" scroll-y>
      <div class="categories-right-item"  v-for="item in secondLevelCategories" :key="item.cat_id">
        <div class="categories-right-item-title">
        <text> {{item.cat_name}}</text>
        </div>
         <div class="categories-right-item-body">
        <view class="right-body-item" v-for="(item1,index2) in item.children" :key="item1.cat_id">
          <!-- 使用要绑定，单标签记得加上结束符 -->
          <img :src="item1.cat_icon" alt=""/>
               <text class="right-body-item-title"> {{item1.cat_name}}</text>
           </view>
            </div>
            </div>
 </scroll-view >

  </div>
  </div>
</template>


<script>
export default {
   data(){
     return {
  categories:[],  //左边内容
  indexasd:0,   //选中的下标
  secondLevelCategories:[], //右边内容
  windowHeight:0   //高度
     }
   },
   onLoad(){
          this.categoy()
          // 获取可使用窗口高度
          const {windowHeight}=wx.getSystemInfoSync()
            console.log(windowHeight);
            // 解决双滚动问题
          this.windowHeight = windowHeight - 10
   },
   methods:{
        async categoy(){
           const res=await this.$axios.get("categories")
           this.categories=res.data.message
            this.secondLevelCategories = this.categories[0].children  //默认选中

        },
        catdian(index){
           this.indexasd=index   //选中的下标
              this.secondLevelCategories = this.categories[index].children
        }
   },
}
</script>

<style lang='less' scoped>
.categories {
  display: flex;
  margin-top: 10px;
  &-left {
    width: 200rpx;
    display: flex;
    flex-direction: column;
    // justify-content: center;
    align-items: center;
    background-color: #f4f4f4;
    &-item {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 200rpx;
      height: 100rpx;
      position: relative;
      color:#999;
    }
    &-active {
      background: #fff;
      color:#000;
      &::before {
        position: absolute;
        content: '';
        background-color: #ff2d4a;
        left: 0;
        top: 10rpx;
        width: 5px;
        height: 80rpx;
      }
    }
  }
  &-right {
    flex: 1;
    background-color: #fff;
    &-item {
      &-title {
        display: flex;
        justify-content: center;
        align-items: center;
        color:#666;
        font-size: 14px;
        &::before {
          content: '/';
          color: #eeeeee;
          margin-right: 20rpx;
        }

        &::after {
          content: '/';
          color: #eeeeee;
          margin-left: 20rpx;
        }
      }
      &-body {
        display: flex;
        flex-wrap: wrap;
      }
    }
  }
}
image {
  width: 100rpx;
  height: 80rpx;
}
.right-body-item {
  height: 200rpx;
  width: 33.33333%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  &-title{
    margin-top:5rpx;
    color:#666;
    font-size: 14px;
  }
}
</style>

