<template>
	<view class="input_view">
		<view class="input_title">请输入题解</view>
		<view class="input_class_view">
			<textarea class="input_class" @input="radioGroupChange" focus placeholder="请输入题解" />	
		</view>

		<button class="tijiao"  @tap="TijiaoClick" >
			提交题解
		</button>
	</view>


</template>

<script>
	export default {
		data() {
			return {
				id:0,
				index:'',
				recookie : '',
			}
		},
		methods: {
			radioGroupChange(e) {
				this.index=e.detail.value
			},
			TijiaoClick(e){
				uni.showModal({
					content:"确认提交题解",
					confirmText:"确定",
					cancelText:"取消",
					success:function(res){
						if(res.confirm){
							console.log(this.index);
							uni.request({
								url: 'http://122.9.32.180/api/user/solution' ,
								data: {
									id: this.id * 1,
									solution :this.index,
								},
								method: 'POST',
								header: {
									Cookie: this.recookie,
								},
								success:(res)=>{
									console.log(res)
								},
								fail:(res)=>{
									console.log(res)
								},
							})
							uni.navigateBack({
								delta:'1'
							})
						}
					}.bind(this)
				})
			},
		},
		onLoad:function(options){
			this.id = options.id,
			this.recookie = uni.getStorageSync('Cookie');
			console.log(this.id)
		},
	}
</script>

<style>
	.input_class{
		background-color: #EBEBEB;
		justify-content: space-between;
		margin-top: 10px;
		margin-left: 10px;
		margin-right: 10px;
		border-radius: 5px;
	}
	.input_view{
		position: relative;
		justify-content: center;
		border-radius: 5px;
	}
	.input_title{
		text-align: center;
		border-radius: 5px;
		display: block;
		
	}
	.tijiao{
		margin-left: 20px;
		margin-right: 20px;
		margin-top: 20px;
		background-color: #EBEBEB;
		border-radius: 5px;
	}
	.input_class_view{
		flex-direction: row;
		width: 100%;
		align-items: center;
	}
</style>
