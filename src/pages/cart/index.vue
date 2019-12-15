<template>
  <div class="wrap">
    <div class="main">

      <div class="item" v-for="(item,index) in book" wx:key="index">
        <div class="top">
          <div class="left">
            <checkbox :checked="item.check" @click="checkOne(index)"></checkbox>
            <img :src="item.image" alt class="img" />
          </div>
          <div class="right">
            <div class="right-top">
              <p>{{item.name}}</p>
              <p @click="del(index)">删除</p>
            </div>
            <div class="right-bottom">
              <p class="price">
                <span class="span-left">￥{{item.price}}</span>
                <span class="span-right">￥{{item.old_price}}</span>
              </p>
              <p class="btns">
                <button @click="reduce(index)">-</button>
                <span>{{item.count}}</span>
                <button @click="add(index)">+</button>
              </p>
            </div>
          </div>
        </div>
        <div class="bottom">
          <p class="p1">
            <span class="jiagou">加购价</span>
            <span>购买一件，即可享受换购优惠</span>
          </p>
          <p class="p2">去选择></p>
        </div>
      </div>

    </div>
    <div class="total">
      <checkbox :checked="ischeckAll" @click="checkAll">全选</checkbox>
      <span>合计：￥{{total}}</span>
      <p class="sum">结算({{cd}})</p>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      book: [],
      ischeckAll: ""
    };
  },

  methods: {
    add(i) {
      this.book[i].count++;
      wx.setStorageSync("book", this.book);
    },
    reduce(i) {
      if (this.book[i].count > 1) {
        this.book[i].count--;
      }
      wx.setStorageSync("book", this.book);
    },
    //全选
    checkAll() {
      this.ischeckAll = !this.ischeckAll;
      this.book.forEach(item => {
          item.check=this.ischeckAll
        // if (this.ischeckAll) {
        //   item.check = true;
        // } else {
        //   item.check = false;
        // }
      });
    },
    //单选
    checkOne(index) {
      this.book[index].check = !this.book[index].check;
      wx.setStorageSync("book", this.book);
      this.isCheck()
    },
    del(index) {
      this.book.splice(index, 1);
      wx.setStorageSync("book", this.book);
    },
    isCheck() {//方便页面加载时调用
      const bool = this.book.every(item => {
        return item.check == true;
      });
      if (bool == true) {
        this.ischeckAll = true;
      } else {
        this.ischeckAll = false;
      }
    }
  },
  computed: {
    total() {
      var sum = 0;
      this.book.forEach(item => {
        if (item.check == true) {
          sum += item.count * item.price;
        }
      });
      return sum;
    },
    cd() {
      const arr = this.book.filter(item => {
        return item.check == true;
      });
      return arr.length;
    }
  },

  created() {},
  onShow() {
    const book = wx.getStorageSync("book")||[];
    this.book = book;
    this.isCheck()
  }
};
</script>
<style scoped>
.wrap {
  background: #f3f3f3;
  padding: 20rpx;
}
.img {
  width: 166rpx;
  height: 180rpx;
  vertical-align: middle;
}
.item {
  padding: 20rpx;
  width: 708rpx;
  height: 336rpx;
  background: #fff;
  margin-bottom: 20rpx;
  border-radius: 20rpx;
  box-sizing: border-box;
}
.top {
  display: flex;
  justify-content: space-around;
}
.left {
  width: 260rpx;
  height: 220rpx;
}
.right {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.right-top {
  font-weight: 700;
  display: flex;
  justify-content: space-between;
}
button {
  width: 40rpx;
  height: 40rpx;
  border-radius: 50%;
  text-align: center;
  line-height: 40rpx;
  padding: 0;
  margin: 0 20rpx;
}
.right-bottom {
  display: flex;
  justify-content: space-between;
}
.btns {
  display: flex;
}
.span-left {
  margin-right: 30rpx;
  color: red;
}
.span-right {
  color: #666;
  text-decoration: line-through;
  font-size: 24rpx;
}
.bottom {
  display: flex;
  font-size: 28rpx;
  justify-content: space-between;
  padding: 20rpx 0;
  padding-left: 50rpx;
}
.jiagou {
  border: 1px solid red;
  color: red;
}
.total {
  display: flex;
  width:100%;
  height: 94rpx;
  line-height: 94rpx;
  justify-content: space-around;
  background: #fff;
  /* position: fixed;
  bottom:0; */
  border-bottom:20rpx solid #fff;
  /* left: 0; */
}
.sum {
  width: 236rpx;
  height: 94rpx;
  background: red;
  color: #fff;
  text-align: center;
  line-height: 94rpx;
  border-radius: 50rpx;
  font-weight: 700;
}
</style>