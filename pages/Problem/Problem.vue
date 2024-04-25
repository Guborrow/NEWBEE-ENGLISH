<template>
  <view>

    <view class="imagestyle">
		<button @click="handleClick"> 
			<image  src="../../static/my-icons/icon1.png" mode="aspectFit" style="width: 150rpx; height: 140rpx;"></image>
			<rich-text nodes="<h1 style='color: red;display: flex; font-size:20px;justify-content:center'>单项选择</h1>">
			</rich-text>
		</button>
    </view>

    <view class="imagestyle">
		<button @click="wanxingClick">
			<image  src="../../static/my-icons/icon4.png" mode="aspectFit" style="width: 150rpx; height: 140rpx;"></image>
		    <rich-text nodes="<h1 style='color: red;display: flex; font-size:20px;justify-content:center'>完形填空</h1>">
			</rich-text>
		</button>
    </view>

    <view class="imagestyle">
		<button @click="yueduClick">
			<image src="../../static/my-icons/icon1.png" mode="aspectFit" style="width: 150rpx; height: 140rpx;"></image>
			<rich-text nodes="<h1 style='color: red;display: flex; font-size:20px;justify-content:center'>阅读理解</h1>">
			</rich-text>	
		</button>
	</view>



  </view>
</template>

<script>
  export default {
	  
    data() {
      return {

      };
    },
	methods:{
		handleClick(){
			uni.navigateTo({
				url: '../problem_danxuan/problem_danxuan',
				success: res => {},
				fail: () => {},
				complete: () => {}
			});
		},
		yueduClick(){
			uni.navigateTo({
				url: '../problem_yuedu/problem_yuedu',
				success: res => {},
				fail: () => {},
				complete: () => {}
			});
		},
		wanxingClick(){
			uni.navigateTo({
				url: '../problem_wanxing/problem_wanxing',
				success: res => {},
				fail: () => {},
				complete: () => {}
			});
		}
	}
  }
</script>

<style lang="scss">
  
  .imagestyle {
    display: flex;
    justify-content: center;
  }
</style>
