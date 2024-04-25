<template>
  <view>
    <view class="my-userinfo-container">
      <view class="top-box">
        <image :src="avatar" class = "ava"></image>
        <view class="nic" >
          <rich-text :nodes="name" class="nic"></rich-text>
        </view>
      </view>
    </view>
    <view class="panel-list">
      <view class="panel">
        <view class="panel-body">
            <view class="panel-item" @click="click1()">
              <image src="../../static/mistake.jpg" class="icon"></image>
              <rich-text nodes='错题本' class="title"></rich-text>
            </view>
            <view class="panel-item" @click="click2()">
              <image src="../../static/history.jpg" class="icon"></image>
              <rich-text nodes='历史记录' class="title"></rich-text>
            </view>
            <view class="panel-item" @click="click3()">
              <image src="../../static/rank.jpg" class="icon"></image>
              <rich-text nodes='排行榜' class="title"></rich-text>
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
        List: [],
        avatar: '',
        name: ''
      };
    },
    onLoad() {
      this.getavatar(),
      this.getname(),
      this.getList()
    },
    methods:{
      getavatar(){
        let that = this
        uni.getStorage({
          key: 'userinfo',
          success(res)  {
            that.avatar = res.data.avatarUrl
            console.log(that.avatar)
          }
        })
      },
      getname(){
        let that = this
        uni.getStorage({
          key: 'userinfo',
          success: (res) => {
            that.name = res.data.nickName
            console.log(that.name)
          }
        })
      },
      async getList() {
        const {data: res} = await uni.$http.get('/api/user/wrong_que_book')
        if(res.meta.status != 200) return uni.$showMsg()
        this.List = res.message
      },
      
      click1() {
        
           uni.navigateTo({
             url: '../mistakes/mistakes'
           })
        },
       click2() {
           uni.navigateTo({
             url: '../history/history'
           })
        },
      click3() {
           uni.navigateTo({
             url: '../rank/rank'
           })
        
        },
      
      
      
    }
  }
</script>

<style lang="scss">
.my-userinfo-container{
  height: 100%;
  background-color: #f4f4f4;
  .top-box{
    height: 400rpx;
    background-color:#000000;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    .ava{
      width: 90px;
      height: 90px;
      border-radius: 45px;
      border: 3px solid #FFFFFF;
    }
    .nic{
      font-size: 25px;
      color: #FFFFFF;
      font-weight: bold;
      margin-top: 20px;
    }
  }
}
  
.panel-list {
  padding: 0 10px;
  position: relative;
  top: 20px;

  .panel {
    background-color: white;
    border-radius: 5px;
    margin-bottom: 8px;

    .panel-body {
      display: flex;
      justify-content: space-around;

      .panel-item {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-around;
        font-size: 13px;
        padding: 10px 0;

        .icon {
          margin: 20px;
          width: 80px;
          height: 80px;
        }
        
        .title{
          font-size: 25px;
          color: #000000;
          font-weight:bold;
          margin-bottom: 20px;
        }
      }
    }
  }
}
</style>
