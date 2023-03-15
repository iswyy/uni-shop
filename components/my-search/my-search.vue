<template>
  <view class="my-search-container" :style="{'background-color':bgcolor}">
    <!-- 使用view组件模拟输入框，真正的输入框在点击进入搜索页面之后才会出现 -->
    <view class="my-search-box" :style="{'border-radius': radius +'px'}" @click="serchBoxHandler">
      <uni-icons type="search" size="17"></uni-icons>
      <text class="placeholder">搜索</text>
    </view>
    
  </view>
</template>

<script>
  export default {
    name:"my-search",
    data() {
      return {
        
      };
    },
    // 为了增强组件的通用性，允许使用者自定义搜索组件的背景颜色和圆角尺寸
    props:{
      bgcolor:{
        type:String,
        default:'#C00000'
      },
      radius:{
        type:Number,
        default:18
      }
    },
    methods:{
      serchBoxHandler(){
        this.$emit('click')
      }
    }
    
  }
</script>

<style lang="scss">
.my-search-container {
  // background-color: #c00000;
  height: 50px;
  padding: 0 10px;
  display: flex;
  align-items: center;
}

.my-search-box {
  height: 36px;
  background-color: #ffffff;
  // border-radius: 15px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;

  .placeholder {
    font-size: 15px;
    margin-left: 5px;
  }
}
</style>