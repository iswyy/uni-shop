<template>
  <view>
    <!-- 轮播图区域 -->
    <swiper :circular="true" :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
      <swiper-item v-for="(item, i) in swiperList" :key="i" >
      <navigator class="swipe-item" :url="'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id">
        <image :src="item.image_src"></image>
      </navigator>
      </swiper-item>
    </swiper>
    <!-- 分类导航区域 -->
    <view class="nav-list">
      <view class="nav-item" v-for="(item,i) in navList" :key="i" @click="navClickHandler(item)">
        <image :src="item.image_src" class="nav-img"></image>
      </view>
    </view>
    <!-- 楼层区域 -->
    <view class="floor-list">
      <!-- 每个楼层项 -->
      <view class="floor-item" v-for="(item,i) in floorList" :key="i">
        <!-- 楼层标题 -->
        <image class="floor-title" :src="item.floor_title.image_src"></image>
        <!-- 楼层图片区域 -->
        <view class="floor-img-box">
          <!-- 左侧大图片 -->
          <navigator class="left-img-box" :url="item.product_list[0].url">
            <image :src="item.product_list[0].image_src" mode="widthFix" :style="{width: item.product_list[0].image_width + 'rpx'}"></image>
          </navigator>
          <!-- 右侧四个小图片 -->
          <view class="right-img-box">
            <navigator class="right-img-item" v-for="(item2,i2) in item.product_list" :key="i2" v-if="i2 !== 0" :url="item2.url">
              <image :src="item2.image_src" mode="widthFix" :style="{width: item2.image_width + 'rpx'}"></image>
            </navigator>
          </view>
        </view>
        
      </view>
    </view>
    
  </view>
</template>

<script>
  export default {
    data() {
      return {
        swiperList:[],
        navList:[],
        floorList:[]
      };
    },
    onLoad(){
      this.getSwiperList()
        this.getNavList()
        this.getFloorList()
    },
    methods:{
     async getSwiperList(){
        const res= await uni.$http.get('/api/public/v1/home/swiperdata')
        console.log(res)
        if(res.data.meta.status !== 200)  return uni.$showMsg()
        this.swiperList=res.data.message
        // uni.$showMsg("数据请求成功！")
      },
      async getNavList(){
        const res= await uni.$http.get('/api/public/v1/home/catitems')
        console.log(res)
        if(res.data.meta.status !== 200)  return uni.$showMsg()
        this.navList=res.data.message
      },
      async getFloorList(){
        const res= await uni.$http.get('/api/public/v1/home/floordata')
        console.log(res)
         // 通过双层 forEach 循环，处理 URL 地址
         res.data.message.forEach(floor => {
            floor.product_list.forEach(prod => {
              prod.url = '/subpkg/goods_list/goods_list?' + prod.navigator_url.split('?')[1]
            })
          })
        this.floorList=res.data.message
      },
      navClickHandler(item){
        if(item.name == '分类'){
          uni.switchTab({
            url:'/pages/cate/cate'
          })
          
        }
      }
    }
  }
</script>

<style lang="scss">
  swiper {
   height: 330rpx;
  }
  .swipe-item{
     height: 100%;
     width: 100%;
  } 
   swiper image{
    width: 100%;
    height: 100%;
  }
  .nav-list {
    display: flex;
    justify-content: space-around;
    margin: 15px 0;
  
    .nav-img {
      width: 128rpx;
      height: 140rpx;
    }
  }
.floor-title {
  height: 60rpx;
  width: 100%;
  display: flex;
}
.right-img-box {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.floor-img-box {
  display: flex;
  padding-left: 10rpx;
}
</style>
