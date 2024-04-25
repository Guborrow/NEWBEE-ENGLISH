<template>

	<list :id="parentListId" class="page" :bounce="false" isSwiperList="true">
	<view id="head" class="header">
		<text class="header-title"> {{question}}</text>
	</view>
	<view>
			<button :class="is1?'yesclick':'notclick'" @tap="choose_click1" :disabled="disabled">{{items["0"].choose}}{{items["0"].name}}</button>
			<button :class="is2?'yesclick':'notclick'" @tap="choose_click2" :disabled="disabled">{{items["1"].choose}}{{items["1"].name}}</button>
			<button :class="is3?'yesclick':'notclick'" @tap="choose_click3" :disabled="disabled">{{items["2"].choose}}{{items["2"].name}}</button>
			<button :class="is4?'yesclick':'notclick'" @tap="choose_click4" :disabled="disabled">{{items["3"].choose}}{{items["3"].name}}</button>
	</view>

		<block v-if="isshow">
			<button class="tijiao" @tap="TijiaoClick">
			提交
			</button>
		</block>
		<block v-else>
			<view class="answer">
				<label>我的答案：{{value}}</label>
			</view>			
			<view class="answer">
				<label>正确答案：C</label>
			</view>
			<block v-if="istijie">
				<button class="chakantijie" @click="kantijie">
					查看题解
				</button>
			</block>
			<block v-else >
				<view>
					<view class="tijie"> 题解如下</view>
					<view v-for="(item1,index) in listinfo" :key="index">
						<view v-show="isOpen || index < max">
							<view>
								<text class="tijie">
									{{item1.tijie}}
								</text>
							</view>
						</view>
					</view>
					<view class="see_more" v-show="!isOpen && listinfo.length > max" @click="isOpen = !isOpen">
						<text class="see_more_zi">查看更多</text>
					</view>
					<view class="see_more" v-show="isOpen && listinfo.length > max" @click="isOpen = !isOpen">
						收起
					</view>
				</view>

			</block>
			
			<button class="xietijie" @click="gototijie">
				写题解
			</button>
			<button class="nextproblem" @tap = "next_question()">
				下一题
			</button>
		</block>


	</list>
</template>

<script>
	// #ifdef APP-PLUS
	const dom = weex.requireModule('dom');
	const app = getApp();
//	const recookie = uni.getStorageSync('Cookie');
	// #endif
	
	export default {
		data() {
			return {
				parentListId: "parentListId",
				pageHeight: 300,
				dataList: [],
				refreshing: false,
				refreshText: "",
				refreshFlag: false
			}
		},
	data() {
		return {
			recookie : '',
			is1: false,
			is2: false,
			is3: false,
			is4: false,
			max: 1,
			question:'',
			isOpen: false,
			title: 'checkbox 复选框',
				items: [{
					value:'A',
					choose:'A.',
					name:''
				},
				{
					value:'B',
					choose:'B.',
					name:''
				},
				{
					value:'C',
					choose:'C.',
					name:''
				},
				{
					value:'D',
					choose:'D.',
					name:''
				},
			],
		listinfo: [],	
		value:'',
		isshow: true,
		ischoose: false,
		disabled:false,
		istijie:true,
		questionid:0,
		tijie_num: 0,
		};
	},
		methods: {
			choose_click1(){
				if(this.is1 == true)
				{
					this.value = null,
					this.is1 = false,
					this.is2 = false,
					this.is3 = false,
					this.is4 = false					
				}
				else
				{
					this.value = 'A',
					this.is1 = true,
					this.is2 = false,
					this.is3 = false,
					this.is4 = false
				}

			},
			choose_click2(){
				if(this.is2 == true)
				{
					this.value = null,
					this.is1 = false,
					this.is2 = false,
					this.is3 = false,
					this.is4 = false					
				}
				else
				{
					this.value = 'B',
					this.is1 = false,
					this.is2 = true,
					this.is3 = false,
					this.is4 = false
				}
			},
			choose_click3(){
				if(this.is3 == true)
				{
					this.value = null,
					this.is1 = false,
					this.is2 = false,
					this.is3 = false,
					this.is4 = false					
				}
				else
				{
					this.value = 'C',
					this.is1 = false,
					this.is2 = false,
					this.is3 = true,
					this.is4 = false
				}
			},
			choose_click4(){
				if(this.is4 == true)
				{
					this.value = null,
					this.is1 = false,
					this.is2 = false,
					this.is3 = false,
					this.is4 = false					
				}
				else
				{
					this.value = 'D',
					this.is1 = false,
					this.is2 = false,
					this.is3 = false,
					this.is4 = true
				}
			},
			TijiaoClick(e){
				console.log(this.value)
				if(!this.value)
				{
					uni.showToast({
						title:"请作答",
						icon:'exception',
						duration:850,
					});
					console.log(123)
				}
				else
				{
					this.isshow= false
					console.log(this.value + "111")
					this.disabled= true
				}
				
			},
			gototijie(){
				uni.navigateTo({
					url:"../tijie/tijie?id=" + this.questionid
				})
			},
			kantijie(){
				this.istijie=false;
				uni.request({
					url: 'http://122.9.32.180/api/user/solution' ,
					data: {
						id: this.questionid,
					},
					method: 'GET',
					header: {
						Cookie: this.recookie,
					},
					success:(res)=>{
						console.log(res)
						this.tijie_num = res.data["solution_num"],
						this.listinfo.length = 0
						var i=0;
						for(i=0;i<this.tijie_num;i++)
						{
							this.listinfo.push({
								tijie:''
							})
						}
						for(i=0;i<this.tijie_num;i++)
						{
							this.listinfo[i].tijie = res.data["solution"]["content"]
						}
					},
					fail:(res)=>{
						
					},
				})
			},
			next_question(){
				uni.reLaunch({
					url:"../Problem_danxuan/Problem_danxuan"
				})
				// this.reload()
				this.isOpen= false,
				this.isshow= true,
				this.ischoose= false,
				this.disabled=false,
				this.istijie=true,
				this.value = null,
				this.is1 = false,
				this.is2 = false,
				this.is3 = false,
				this.is4 = false,
				uni.request({
					url: 'http://122.9.32.180/api/user/get_question' ,
					data: {
						type :'choice_question',
					},
					method: 'GET',
					header: {
						Cookie: this.recookie,
					},
					success:(res)=>{
						console.log(res)
						console.log(res.data["sub_que"]["0"]["options"]["0"])
						this.question = res.data["sub_que"]["0"]["stem"]
						this.items["0"].name = res.data["sub_que"]["0"]["options"]["0"]
						this.items["1"].name = res.data["sub_que"]["0"]["options"]["1"]
						this.items["2"].name = res.data["sub_que"]["0"]["options"]["2"]
						this.items["3"].name = res.data["sub_que"]["0"]["options"]["3"]
						this.questionid = res.data["sub_que"]["0"]["id"]
						// this.items.forEach((item =>{
						// 	item.checked = ""
						// }))
					},
					fail(res){
						console.log('fail')
					}
				})
			},

		},
		onLoad(e){
				console.log('test');
				//清空check属性
				this.recookie = uni.getStorageSync('Cookie');
				uni.request({
					url: 'http://122.9.32.180/api/user/get_question' ,
					data: {
						type :'choice_question',
					},
					method: 'GET',
					header: {
						Cookie: this.recookie,
					},
					success:(res)=>{
						console.log(res)
						console.log(res.data["sub_que"]["0"]["options"]["0"])
						this.question = res.data["sub_que"]["0"]["stem"]
						this.items["0"].name = res.data["sub_que"]["0"]["options"]["0"]
						this.items["1"].name = res.data["sub_que"]["0"]["options"]["1"]
						this.items["2"].name = res.data["sub_que"]["0"]["options"]["2"]
						this.items["3"].name = res.data["sub_que"]["0"]["options"]["3"]
						 this.items["0"].checked = ''
						 this.items["1"].checked = ''
						 this.items["2"].checked = ''
						 this.items["3"].checked = ''
						this.questionid = (res.data["sub_que"]["0"]["id"])*1
						
						console.log(this.items)
						// this.items.forEach((item =>{
						// 	item.checked = false
						// }))
					},
					fail(res){
						console.log('fail')
					}
				})
		/*		获取答案
				uni.request({
					url: 'http://122.9.32.180/api/user/check_question' ,
					data: {
						type :'CHOICE_QUE_NAME',
					},
					method: 'GET',
					header: getApp().globalData.header,
					success:(res)=>{
						console.log(res)
					},
					fail(res){
						console.log('fail')
					}
				})*/
		}
	}
</script>

<style>
	/* #ifndef APP-PLUS */
	page {
		width: 100%;
		min-height: 100%;
		display: flex;
	}

	/* #endif */

	.page {
		flex: 1;
	}

	.refresh-view {
		width: 750rpx;
		height: 80px;
		flex-direction: row;
		align-items: center;
		justify-content: center;
	}

	.header {
		margin-left: 20px;
		margin-right: 20px;
		flex-direction: row;
		align-items: center;
		justify-content: center;
		background-color: #f4f4f4;
	}

	.header-title {
		font-size: 25px;
		font-weight: bold;
		color: #444;
		margin-left: 12px;
		margin-right: 12px;
	}

	.sticky-view {
		margin-left: 12px;
		margin-right: 12px;
		padding: 20px;
		background-color: #EBEBEB;
		border-radius: 5px;
	}

	.list {
		flex: 1;
	}

	.list-item {
		margin-left: 12px;
		margin-right: 12px;
		margin-top: 12px;
		padding: 20px;
		background-color: #fff;
		border-radius: 5px;
	}
	.tijiao{
		margin-left: 20px;
		margin-right: 20px;
		background-color: #EBEBEB;
		border-radius: 5px;
	}		
	.radio_class{
		margin-left: 12px;
		padding: 12px;
	}
	.name_class{
		margin-left: 0px;
		margin-right: 12px;
	}
	.answer{
		margin-left: 20px;
	}
	.kantijie{
		margin-left: 20px;
	}
	.chakantijie{
		margin-left: 20px;
		margin-right: 20px;
		border-radius: 5px;
	}
	.xietijie{
		width: 45% !important;
		margin-right: 12px;
		float:right;
		margin-top: 10px;
		font-weight:100;
	}
	.nextproblem{
		width: 45% !important;
		float:left;
		margin-left: 12px;
		margin-top: 10px;
		font-weight:100;
	}
	.tijie{
		margin-left: 20px;
		margin-right: 20px;
		border-radius: 10px;
		background-color: #ffffff;
	}
	.see_more{
		margin-left: 20px;
		margin-right: 20px;
		border-radius: 10px;
		background-color: #ffffff;
		padding: 5px;
		flex-direction: row;
		align-items: center;
	}
	.notclick{
		margin-left: 20px;
		margin-right: 20px;
		background-color: #ffffff;
		border-radius: 5px;
	}
	.yesclick{
		margin-left: 20px;
		margin-right: 20px;
		background-color: #55ff7f;
		border-radius: 5px;
	}
</style>
