<template>
	<view>
		<uni-section title="错题本" type="line">
      
			<view class="uni-padding-wrap uni-common-mt">
				<uni-segmented-control :current="current" :values="items" :style-type="styleType"
					:active-color="activeColor" @clickItem="seg_onClickItem" />
			</view>
      

				<view v-if="current === 0">
          	<uni-swipe-action ref="swipeAction">
          		<uni-swipe-action-item
          		    v-for="(item, index) in swipeList"
          		    :right-options="item.options"
          		    :key="item.id"
          		    @click="swipeClick($event, index)"
          		>
          			<button class="content-box" @click="click_nav(index)">
          				<text class="content-text">{{ item.content }}</text>
          			</button>
          		</uni-swipe-action-item>
          	</uni-swipe-action>
            <uni-section padding = 25px>
            			<uni-pagination :total="11" 
                  title="标题文字" 
                  pageSize="10"
                  @change = "page_change">
                  </uni-pagination>
            </uni-section>
          </view>
        
        
				<view v-if="current === 1">
          <view class="container">
          	<uni-section
          	    title="历史记录"
          	    type="line"
          	></uni-section>
          	<uni-swipe-action ref="swipeAction">
          		<uni-swipe-action-item
          		    v-for="(item, index) in swipeList"
          		    :right-options="item.options"
          		    :key="item.id"
          		    @click="swipeClick($event, index)"
          		>
          			<button class="content-box" @click="click_nav(index)">
          				<text class="content-text">{{ item.content }}</text>
          			</button>
          		</uni-swipe-action-item>
          	</uni-swipe-action>
            <uni-section padding = 25px>
            			<uni-pagination :total="11" 
                  title="标题文字" 
                  pageSize="10"
                  @change = "page_change">
                  </uni-pagination>
            </uni-section>
          </view>
        </view>
        
        
				<view v-if="current === 2">
          <view class="container">
          	<uni-section
          	    title="历史记录"
          	    type="line"
          	></uni-section>
          	<uni-swipe-action ref="swipeAction">
          		<uni-swipe-action-item
          		    v-for="(item, index) in swipeList"
          		    :right-options="item.options"
          		    :key="item.id"
          		    @click="swipeClick($event, index)"
          		>
          			<button class="content-box" @click="click_nav(index)">
          				<text class="content-text">{{ item.content }}</text>
          			</button>
          		</uni-swipe-action-item>
          	</uni-swipe-action>
            <uni-section padding = 25px>
            			<uni-pagination :total="11" 
                  title="标题文字" 
                  pageSize="10"
                  @change = "page_change">
                  </uni-pagination>
            </uni-section>
          </view>
        
			</view>
		</uni-section>

		
	</view>
</template>

<script>
	export default {
		components: {},
		data() {
			return {
        show: false,
        isOpened: 'none',
        swipeList: [],
        id: [],
				items: ['单项选择', '完型填空', ' 阅读理解'],
				styles: [{
						value: 'button',
						text: '按钮',
						checked: true
					},
					{
						value: 'text',
						text: '文字'
					}
				],
				colors: ['#007aff', '#4cd964', '#dd524d'],
				current: 0,
				colorIndex: 0,
				activeColor: '#007aff',
				styleType: 'button'
			}
		},
    mounted() {
    	setTimeout(() => {
    		this.current = 5
    	}, 3000)
    },
    onReady() {
      let that = this
      let arraytemp = [];
        for(let i = 0; i <  10; i++){
          let ttmp = {}
          ttmp.options = [{text: '删除',style: {backgroundColor: 'rgb(255,58,49)'}}],
          ttmp.num = i,
          ttmp.content= 'item' + (i+1), //此处为题目标题
          ttmp.id = '19182635'+i,
          arraytemp.push(ttmp),
          that.$data.id[ttmp.num + 1] = ttmp.id
        }
      that.$data.swipeList = arraytemp 
    	// 模拟延迟赋值
    	setTimeout(() => {
    		this.isOpened = 'right';
    	}, 1000);
    	
    	uni.$on('update',res=>{
    		console.log(111);
    		this.swipeClick({
    			content:{
    				text:'添加'
    			}
    		})
    	})
    },
		methods: {
			seg_onClickItem(e) {
				if (this.current !== e.currentIndex) {
					this.current = e.currentIndex
				}
			},
			styleChange(e) {
				if (this.styleType !== e.detail.value) {
					this.styleType = e.detail.value
				}
			},
			colorChange(e) {
				if (this.styleType !== e.detail.value) {
					console.log(e.detail.value);
					this.activeColor = e.detail.value
				}
			},
      page_change(e) {
      	console.log(e)
      	this.current = e.current
        this.type = e.type
        console.log(this.type)
        if(this.type == 'next'){
          uni.navigateTo({
            url: '../mistakes/mistakes'
          })
        }
        else if(this.type == 'prev'){
          uni.navigateTo({
            url: '../rank/rank'
          })
        }
      },
      click_nav(index){
        let pos = index + 1;
        let that = this;
        let trans_id = that.$data.id[pos];
        //console.log(trans_id);
        try{
          uni.setStorageSync('nav_id',trans_id);
        }catch(e){
          //TODO handle the exception
          "store_id_failure"
        }
        uni.navigateTo({
          url: '../display/display'
        })
      },
      swipeClick(e, index) {
      	let {
      		content
      	} = e;
        //此处获得由swiperlist传递的题目ID，存入storage
      	if (content.text === '删除') {
      		uni.showModal({
      			title: '提示',
      			content: '是否删除',
      			success: res => {
      				if (res.confirm) {
      					this.swipeList.splice(index, 1);
      				} else if (res.cancel) {
      					console.log('用户点击取消');
      				}
      			}
      		});
      	} else {
      		uni.showToast({
      			title: `点击了${e.content.text}按钮`,
      			icon: 'none'
      		});
      	}
      }
		}
	}
</script>

<style lang="scss">
	.example-body {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		padding: 0;
	}

	.uni-common-mt {
		margin-top: 30px;
	}

	.uni-padding-wrap {
		// width: 750rpx;
		padding: 0px 30px;
	}

	.seg_content {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		justify-content: center;
		align-items: center;
		height: 100%;
    weight: 100%;
		text-align: center;
	}

	.seg_content-text {
		font-size: 14px;
		color: #666;
	}

	.color-tag {
		width: 25px;
		height: 25px;
	}

	.uni-list {
		flex: 1;
	}

	.uni-list-item {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex: 1;
		flex-direction: row;
		background-color: #FFFFFF;
	}


	.uni-list-item__container {
		padding: 12px 15px;
		width: 100%;
		flex: 1;
		position: relative;
		/* #ifndef APP-NVUE */
		display: flex;
		box-sizing: border-box;
		/* #endif */
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		border-bottom-style: solid;
		border-bottom-width: 1px;
		border-bottom-color: #eee;
	}

	.uni-list-item__content-title {
		font-size: 14px;
	}
  .content-box {
  		flex: 1;
  		/* #ifdef APP-NVUE */
  		justify-content: center;
  		/* #endif */
  		height: 44px;
  		line-height: 44px;
      weight: 100%;
  		padding: 0 15px;
  		position: relative;
  		background-color: #fff;
  		border-bottom-color: #f5f5f5;
  		border-bottom-width: 1px;
  		border-bottom-style: solid;
  	}
    .container{
      height: 100%;
      weight: 600px;
    }
  	.content-text {
      display: flex;
      justify-content: flex-start;
  		font-size: 15px;
  	}
  
  	.example-body {
  		/* #ifndef APP-NVUE */
  		display: flex;
  		/* #endif */
  		flex-direction: row;
  		justify-content: center;
  		padding: 10px 0;
  		background-color: #fff;
  	}
  .example-body {
  		/* #ifndef APP-NVUE */
  		display: block;
  		/* #endif */
  	}
  
  	.btn-view {
  		/* #ifndef APP-NVUE */
  		display: flex;
  		flex-direction: column;
  		/* #endif */
  		padding: 15px;
  		text-align: center;
  		background-color: #fff;
  		justify-content: center;
  		align-items: center;
  	}
  
  	.btn-flex {
  		display: flex;
  		flex-direction: row;
  		justify-content: center;
  		align-items: center;
  	}
  
  	.button {
  		margin: 20px;
  		width: 150px;
  		font-size: 14px;
  		color: #333;
  	}
</style>
