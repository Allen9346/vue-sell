<template>
  <div class="shopcart">
    <div class="content" @click="toggleList">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount>0}">
            <i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalCount>0}">{{totalPrice}}元</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" @click.stop.prevent="pay">
        <div class="pay" :class="payClass">
          {{payDesc}}
        </div>
      </div>
    </div>
    <div class="shopcart-list" v-show="ListShow">
      <div class="list-header">
        <h1 class="title">购物车</h1>
        <span class="empty" @click="empty">清空</span>
      </div>
      <div class="list-content" ref="listcontent">
        <ul>
          <li class="food border-1px" v-for="food in selectFoods">
            <span class="name">{{food.name}}</span>
            <div class="price">
              <span>￥{{food.price*food.count}}</span>
            </div>
            <div class="cartcontrol-wrapper">
              <cartcontrol :food="food"></cartcontrol>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <div class="list-mask" v-show="bgShow" @click="hideBg"></div>
  </div>
</template>

<script>
import cartcontrol from '../../components/cartcontrol/cartcontrol'
import BScroll from 'better-scroll'

export default {
  props: {
    selectFoods: {
      type: Array,
      default () {
        return [
          {
            price: 10,
            count: 0
          }
        ]
      }
    },
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {
      fold: false
    }
  },
  methods: {
    toggleList () {
      if (!this.totalCount) {
        return
      }
      this.fold = !this.fold
    },
    empty () {
      this.selectFoods.forEach((food) => {
        food.count = 0
      })
    },
    hideBg () {
      this.fold = !this.fold
    },
    pay () {
      if (this.totalPrice < this.minPrice) {
        return
      }
      window.alert(`您需要支付${this.totalPrice}元`)
    }
  },
  computed: {
    totalPrice () {
      let total = 0
      this.selectFoods.forEach((food) => {
        total += food.price * food.count
      })
      return total
    },
    totalCount () {
      let count = 0
      this.selectFoods.forEach((food) => {
        count += food.count
      })
      return count
    },
    payDesc () {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}元起送`
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice
        return `还差￥${diff}元起送`
      } else {
        return '去结算'
      }
    },
    payClass () {
      if (this.totalPrice < this.minPrice) {
        return 'not-enough'
      } else {
        return 'enough'
      }
    },
    ListShow () {
      if (!this.totalCount) {
        this.fold = true
        return false
      }
      let show = !this.fold
      if (show) {
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.listcontent, {
              click: true
            })
          } else {
            this.scroll.refresh()
          }
        })
      }
      return show
    },
    bgShow () {
      let bg = this.ListShow
      return bg
    }
  },
  components: {
    cartcontrol
  }
}
</script>

<style lang="less" rel="stylesheet/less">
@import '../../common/less/mixin.less';

.shopcart{
  position: fixed;
  left: 0;
  bottom: 0;
  z-index: 50;
  width: 100%;
  height: 48px;
  .content{
    display: flex;
    background-color: #141d27;
    color: rgba(255,255,255,0.4);
    font-size: 0px;
    .content-left{
      flex: 1;
      .logo-wrapper{
        display: inline-block;
        position: relative;
        top: -10px;
        margin: 0 12px;
        padding: 6px;
        width: 56px;
        height: 56px;
        box-sizing: border-box;
        vertical-align: top;
        border-radius: 50%;
        background-color: #141d27;
        .logo{
          width: 100%;
          height: 100%;
          border-radius: 50%;
          background-color: #2b3d3c;
          text-align: center;
          &.highlight{
            background-color: rgb(0,160,220);
          }
          .icon-shopping_cart{
            line-height:44px;
            font-size: 24px;
            color: #80858a;
            &.highlight{
              color: #fff;
            }
          }
        }
        .num{
          position: absolute;
          top: 0;
          right: 0;
          width: 24px;
          height: 16px;
          line-height: 16px;
          text-align: center;
          border-radius: 16px;
          font-size: 9px;
          font-weight: 700;
          color: #fff;
          background-color: rgb(240,20,20);
          // h-length, v-length, blur, spread, colour
          box-shadow: 0 4px 8px 0 rgba(0, 0, 0, .4);
        }
      }
      .price{
        display: inline-block;
        vertical-align: top;
        line-height: 24px;
        margin-top: 12px;
        box-sizing: border-box;
        padding-right: 12px;
        border-right: 1px solid rgba(255,255,255,0.1);
        font-size: 16px;
        font-weight: 700;
        &.highlight{
          color: #fff;
        }
      }
      .desc{
        display: inline-block;
        vertical-align: top;
        line-height: 24px;
        margin: 12px 0 0 12px;
        font-size: 10px;
        font-weight: 200;
      }
    }
    .content-right{
      flex: 0 0 105px;
      width: 105px;
      .pay{
        height: 48px;
        line-height: 48px;
        text-align: center;
        font-size: 12px;
        font-weight: 700;
        &.not-enough{
          background-color: #2b333b;
        }
        &.enough{
          background-color: #00b43c;
          color: #fff;
        }
      }
    }
  }
  .shopcart-list{
    position: absolute;
    top: 0;
    left: 0;
    z-index: -1;
    width: 100%;
    transform: translateY(-100%);
    .list-header{
      height: 40px;
      line-height: 40px;
      padding: 0 18px;
      background-color: #f3f5f7;
      border-bottom: 1px solid rgba(7,17,27,0.1);
      .title{
        float: left;
        font-size: 14px;
        color: rgb(7,17,27);
      }
      .empty{
        float: right;
        font-size: 12px;
        color: rgb(0,160,220);
      }
    }
    .list-content{
      padding: 0 18px;
      max-height: 217px;
      background-color: #ffffff;
      overflow: hidden;
      .food{
        position: relative;
        padding: 12px 0;
        box-sizing: border-box;
        .border-1px(rgba(7,17,27,0.1));
        .name{
          line-height: 24px;
          font-size: 14px;
          color: rgb(7,17,27);
        }
        .price{
          position: absolute;
          right: 90px;
          bottom: 12px;
          line-height: 24px;
          font-weight: 700;
          font-size: 14px;
          color: rgb(240,20,20);
        }
        .cartcontrol-wrapper{
          position: absolute;
          bottom: 6px;
          right: 0;
        }
      }
    }
  }
  .list-mask{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -2;
    backdrop-filter: blur(10px);
    background-color: rgba(7,17,27,0.6);
  }
}
</style>
