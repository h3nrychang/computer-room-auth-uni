<template>
  <view class="page">
    <view class="custom-header">审批</view>
    <view class="main-content">
      <text>这里是审批页面的内容</text>
    </view>
  </view>
</template>

<script>
export default {
  name: 'ApprovePage'
}
</script>

<style scoped>
	.page {
	  padding-top: 80rpx; /* 留出固定标题栏的高度 */
	  height: 100vh;
	  box-sizing: border-box;
	}

	.custom-header {
	  position: fixed;
	  top: 0;
	  left: 0;
	  width: 100%;
	  height: 70rpx;
	  padding-top: 20rpx;
	  background-color: #007aff;
	  color: #fff;
	  text-align: center;
	  font-size: 32rpx;
	  line-height: 50rpx;
	  font-weight: bold;
	  z-index: 1000;
	}
	
	.main-content {
	  padding: 30rpx;
	}
</style>