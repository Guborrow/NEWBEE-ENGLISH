<template>
  <view>
    <swiper class="swiper-container" indicator-dots indicator-color="grey" indicator-active-color="white" autoplay="true" interval="2500" circular="true">
      <swiper-item>
        <view class="scrollstyle">
          <image src="../../static/scroll2.jpg" mode = "aspectFit"></image>
        </view>
      </swiper-item>
      <swiper-item>
        <view class="scrollstyle">
          <image src="../../static/scroll1.jpg" mode = "aspectFit"></image>
        </view>
      </swiper-item>
      <swiper-item>
        <view class="scrollstyle">
          <image src="../../static/scroll3.jpg" mode = "aspectFit"></image>
        </view>
      </swiper-item>
    </swiper>
    <view class="billbord">
      <view>
        <rich-text nodes="<h1 style='color: white;display: flex; font-size:30px;justify-content: center; align-items:flex-start'>公告栏</h1>"></rich-text>
        <rich-text  class="notice_class" :nodes=notice ></h1>"></rich-text>
      </view>
      
    </view>
  </view>
  
</template>

<script>
  export default {
    data() {
      return {
		recookie : '',
		notice:'',
      };
    },
	onShow(e){
		console.log('onshow')
		  uni.request({
		  	url: 'http://122.9.32.180/api/user/notice' ,
		  	method: 'GET',
			header: {
				Cookie: this.recookie
			},
		  	success:(res)=>{
				console.log(res)
				this.notice = res.data.content
		  	},
		  	fail(res){
		  		console.log('fail')
		  	}
		  })
	},
	onLoad(){
		this.recookie = uni.getStorageSync('Cookie');
	},
  }

</script>


<style lang="scss">
  .swiper-container{
    height: 500rpx;
  }
  .scrollstyle{
    display: flex;
    justify-content: center;
  }
  .billbord view{
    margin-top: 50rpx;
    width: 800rpx;
    height: 600rpx;
    text-align: center;
    line-height: 100rpx;
  }
  .billbord view:nth-child(1) {
    background-color: black;
  }
  .notice_class{
	  color: white;
	  display: flex; 
	  font-size:18px;
	  justify-content:space-around;
  }
</style>
