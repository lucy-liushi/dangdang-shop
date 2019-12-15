<template>
  <div class="wrap">
    <div class="header">
      <img class="logo" src="/static/images/logo.png" alt />
      <input type="text" class="input" placeholder="超品日，3折封顶" />
      <img class="logo" src="/static/images/class.png" alt />
    </div>
    <swiper autoplay="true" indicator-dots="true" interval="2000" circular="true" class="swiper">
      <swiper-item v-for="(item,index) in imgList" wx:key="index">
        <img :src="item" class="banner" />
      </swiper-item>
    </swiper>
    <div>
      <img v-for="(item,index) in itemList" wx:key="index" :src="item.image" alt class="itemimg" />
    </div>
    <img src="/static/images/pic1.jpg" alt class="pic1" />

    
    <div class="main">
      <div class="item-main" v-for="(item,index) in mainList" wx:key="index" @click="jump(item._id)">
          <img :src="item.image" alt class="main-img" />
          <p class="title">{{item.name}}</p>
          <p class="p">
            <span class="ziying">当当自营</span>
            <span class="jian">满减</span>
          </p>
          <p class="price">￥{{item.price}}</p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      imgList: [
        "/static/images/1.jpg",
        "/static/images/2.jpg",
        "/static/images/3.jpg",
        "/static/images/4.jpg",
        "/static/images/5.jpg",
        "/static/images/6.jpg"
      ],
      itemList: [],
      mainList: []
    };
  },

  methods: {
    jump(id){
      wx.navigateTo({
        url: '/pages/detail/main?id='+id,
        success: (res)=>{
          // console.log(res)
        }
      });
    }
  },
  mounted() {
    wx.request({
      // url: "http://127.0.0.1:8080/book.json",
      url:"http://localhost:3001/",
      success: res => {
        // console.log(res);
        this.mainList = res.data;
      }
    });
    wx.request({
      // url: "http://127.0.0.1:8080/type.json",
      url:"http://localhost:3001/listtype",
      success: res => {
        // console.log(res);
        this.itemList = res.data;
      }
    });
  },

  created() {}
};
</script>
<style scoped>
.header {
  padding: 0 20rpx;
  height: 85rpx;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.logo {
  width: 66rpx;
  height: 50rpx;
}
.input {
  width: 528rpx;
  height: 60rpx;
  background: #e8ebf0;
  border-radius: 30rpx;
  padding-left: 30rpx;
}
.swiper {
  height: 220rpx;
}
.banner {
  width: 100%;
  height: 220rpx;
}
.itembox {
  display: flex;
}
.itemimg {
  width: 150rpx;
  height: 153rpx;
}
.pic1 {
  width: 100%;
  height: 207rpx;
}
.p {
  line-height: 60rpx;
}
.p > span {
  border: 1px solid red;
  margin-right: 20rpx;
  border-radius: 5rpx;
  font-size: 26rpx;
}
.ziying {
  color: red;
}
.jian {
  color: #fff;
  background: red;
}
.main {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
.item-main {
  box-sizing: border-box;
  width: 48%;
  padding: 10rpx;
}
.main-img {
  width: 370rpx;
  height: 378rpx;
}
.title {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  overflow: hidden;
  width: 370rpx;
}
.price {
  color: red;
  line-height: 60rpx;
}
</style>