<template>
  <view>
    <view class="goods-list">
      <view v-for="(item,i) in goodsList" :key="i" @click="gotoDetail(item)">
      <my-goods :goods="item"></my-goods>
      </view>
    </view>
    
  </view>
</template>

<script>
  export default {
    data() {
      return {
        // 请求参数对象
        queryObj:{
          // 查询关键词
          query:'',
          // 商品分类id
          cid:'',
          pagenum:1,
          pagesize:10
        },
        goodsList:[],
        // 总数量
        total:0,
        // 是否正在请求数据
        isloading:false
      };
    },
    onLoad(options){
      // 将页面参数转存到queryObj对象中
      this.queryObj.query=options.query || ''
      this.queryObj.cid=options.cid || ''
      this.getGoodsList()
    },
    methods:{
      
      gotoDetail(item){
        uni.navigateTo({
          url: '/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id
        })
      },
      async getGoodsList(cb){
        this.isloading=true
        const {data:res}=await uni.$http.get('/api/public/v1/goods/search',this.queryObj)
        this.isloading=false
        // 如果cb存在，就调用cb
        cb&& cb()
        if(res.meta.status !==200) return uni.$showMsg()
        this.goodsList=[...this.goodsList,...res.message.goods]
        this.total=res.message.total
        
      }
    },
    onReachBottom(){
      if(this.queryObj.pagenum * this.queryObj.pagesize >= this.total) return uni.$showMsg('数据加载完毕！')
      if(this.isloading) return
      // 页码自增
      this.queryObj.pagenum +=1
      this.getGoodsList()
    },
    // 下拉刷新事件
    onPullDownRefresh(){
      // 重置关键数据
      this.queryObj.pagenum=1
      this.total=0
      this.isloading=false
      this.goodsList=[]
      // 重新发起请求
      this.getGoodsList(() => uni.stopPullDownRefresh())
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
