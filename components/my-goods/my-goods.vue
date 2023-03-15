<template>
        <view class="goods-item">
          <!-- 商品左侧图片区域 -->
          <view class="goods-item-left">
             <image :src="goods.goods_small_logo || defaultPic" class="goods-pic"></image>
          </view>
          <!-- 商品右侧信息区域 -->
          <view class="goods-item-right">
            <!-- 商品标题 -->
            <view class="goods-name">{{goods.goods_name}}</view>
            <view class="goods-info-box">
              <!-- 商品价格 -->
              <view class="goods-price">￥{{goods.goods_price | tofixed}}</view>
            </view>
          </view>
        </view>
</template>

<script>
 export default {
   props:{
     goods:{
       type:Object,
       default:{}
     }
   },
    data() {
      return {
         // 防止某些商品图片不存在所以提前定义一个默认的空图片
            defaultPic: 'https://img3.doubanio.com/f/movie/8dd0c794499fe925ae2ae89ee30cd225750457b4/pics/movie/celebrity-default-medium.png'
        
      };
    },
    filters:{
      // 把价格的数字处理为带两位小数点的数字
      tofixed(num){
        return Number(num).toFixed(2)
      }
    },
    onLoad(options){
      // 将页面参数转存到queryObj对象中
      this.queryObj.query=options.query || ''
      this.queryObj.cid=options.cid || ''
      this.getGoodsList()
    },
    methods:{
      async getGoodsList(){
        const {data:res}=await uni.$http.get('/api/public/v1/goods/search',this.queryObj)
        if(res.meta.status !==200) return uni.$showMsg()
        this.goodsList=res.message.goods
        this.total=res.message.total
        
      }
    }
  }
</script>

<style lang="scss">
.goods-item {
  display: flex;
  padding: 10px 5px;
  border-bottom: 1px solid #f0f0f0;

  .goods-item-left {
    margin-right: 5px;

    .goods-pic {
      width: 100px;
      height: 100px;
      display: block;
    }
  }

  .goods-item-right {
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    .goods-name {
      font-size: 13px;
    }

    .goods-price {
      font-size: 16px;
      color: #c00000;
    }
  }
}
</style>