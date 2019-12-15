<template>
  <div class="wrap">
    <div class="inner">
      <img :src="book.image" class="img" />
      <div class="main">
        <p>
          <span class="ziying">当当自营</span>
          {{book.name}}
        </p>
        <p class="p2">{{book.desc}}</p>
        <p class="p3">
          <span class="price">￥{{book.price}}</span>
          <span class="span2">
            <img src="/static/images/jiangjia.png" alt class="jiangjia" />降价通知
          </span>
        </p>
        <p class="p4">定价￥{{book.youfei}}</p>
      </div>
    </div>
    <div class="footer">
      <div class="left">
        <div>
          <img class="limg" src="/static/images/dianpu.png" alt />
          <p>店铺</p>
        </div>
        <div @click="toCart">
          <img class="limg" src="/static/images/cart.png" alt />
          <p>购物车</p>
        </div>
      </div>
      <div class="right">
        <p class="mai">立即购买</p>
        <p class="add" @click="addCart(book)">加入购物车</p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      book: {}
    };
  },

  methods: {
    addCart(e) {
      console.log(e)
      let pro = wx.getStorageSync("book") || [];
      console.log(pro);
      let index = pro.findIndex(item => {
        return item._id == e._id;
      });
      // 如果缓存数组里面没有，则添加到数组里
      if (index == -1) {
        this.book.count = 1;
        pro.push(this.book);
      } else {//数组缓存里已将存在，则数量增加
        pro[index].count += 1;
      }
      wx.showToast({
          title: '加入购物车成功',
          icon: 'success',
          duration: 2000
        })
      this.book.check=true
      wx.setStorageSync("book", pro);
    },
    toCart(){
      wx.reLaunch({
        url: '/pages/cart/main',
        success: (result)=>{
          // console.log(result)
        }
      });
    }
  },

  created() {
   
  },
  onLoad(e) {
    let id = e.id;
    wx.request({
      // url: "http://127.0.0.1:8080/book.json",
      url: "http://localhost:3001/detail?id="+e.id,
      success: res => {
        this.book = res.data;
      }
    });
  }
};
</script>
<style scoped>


.img {
  width: 100%;
  height: 750rpx;
  border-bottom: 1px solid #ccc;
}
.main {
  padding:0 20rpx;
}
.ziying {
  background: red;
  margin-right: 20rpx;
  border-radius: 5rpx;
  font-size: 26rpx;
  color: #fff;
  border-radius: 20rpx;
  padding: 5rpx;
}
.p2 {
  font-size: 26rpx;
  color: #666;
  line-height: 50rpx;
}
.jiangjia {
  width: 40rpx;
  height: 40rpx;
}
.price {
  color: red;
  font-size: 50rpx;
  margin-right: 30rpx;
}
.p3 {
  line-height: 75rpx;
}
.span2 {
  font-size: 26rpx;
  border: 1px solid #666;
  font-weight: 700;
  border-radius: 30rpx;
  text-align: center;
  padding: 6rpx;
}
.p4 {
  color: #999;
  text-decoration: line-through;
}
.limg {
  width: 45rpx;
  height: 45rpx;
}
.footer {
  height: 120rpx;
  text-align: center;
  display: flex;
  border-top: 20rpx solid #f8f8f8;
}
.left {
  display: flex;
  width: 312rpx;
  justify-content: space-around;
  padding: 20rpx 0;
}
.left > p {
  text-align: center;
}
.right {
  flex: 1;
  display: flex;
  justify-content: space-around;
  line-height: 120rpx;
}
.mai {
  width: 216rpx;
  background: #ffbe24;
  color: #fff;
}
.add {
  width: 216rpx;
  background: #f3554a;
  color: #fff;
}
</style>