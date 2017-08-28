<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if='seller.supports' class="support-count" @click="showDetail">
        <span class="count">
          {{seller.supports.length}}个
        </span>
        <i class='icon-keyboard_arrow_right'></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span><i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="auto">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="item in seller.supports">
                <span class="icon" :class="classMap[item.type]"></span>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>
            <div class="title">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail">
          <i class="icon-close"></i>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import star from '../../components/star/star'

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail () {
      this.detailShow = true
    },
    hideDetail () {
      this.detailShow = false
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  components: {
    star
  }
}
</script>

<style lang="less" rel="stylesheet/less">
@import '../../common/less/mixin.less';
.bg-image(@url){
  background-image: url('@{url}@2x.png');
  @media (-webkit-min-device-pixel-ratio:3),(min-device-pixel-ratio:3){
    background-image: url('@{url}@3x.png');
  }
}

.header{
  position: relative;
  background-color: rgba(7,17,27,0.5);
  color: #fff;
  .content-wrapper{
    position: relative;
    padding: 24px 12px 18px 24px;
    font-size: 0;
    .avatar{
      display: inline-block;
      vertical-align: top;
      img{
        border-radius:2px;
      }
    }
    .content{
      display: inline-block;
      margin-left: 16px;
      .title{
        margin: 2px 0 8px 0;
        .brand{
          display: inline-block;
          vertical-align: top;
          width: 30px;
          height: 18px;
          .bg-image('brand');
          background-size: 30px 18px;
          background-repeat: np-repeat;
        }
        .name{
          margin-left: 6px;
          font-size: 16px;
          line-height: 18px;
          font-weight: bold;
        }
      }
      .description{
        font-size: 12px;
        font-weight: 200;
      }
      .support{
        margin: 10px 0 2px 0;
        .icon{
          display: inline-block;
          vertical-align: top;
          width: 12px;
          height: 12px;
          background-size: 12px 12px;
          background-repeat: np-repeat;
          &.decrease{
            .bg-image('decrease_1');
          }
          &.discount{
            .bg-image('discount_1');
          }
          &.guarantee{
            .bg-image('guarantee_1');
          }
          &.invoice{
            .bg-image('invoice_1');
          }
          &.special{
            .bg-image('special_1');
          }
        }
        .text{
          margin-left: 4px;
          font-size: 10px;
          line-height: 12px;
          font-weight: 200;
        }
      }
    }
    .support-count{
      position: absolute;
      right: 12px;
      bottom: 18px;
      height: 24px;
      padding: 0 8px;
      line-height: 24px;
      border-radius: 14px;
      background-color: rgba(0,0,0,0.2);
      text-align: center;
      .count{
        font-size: 10px;
      }
      .icon-keyboard_arrow_right{
        margin-left: 2px;
        font-size: 10px;
        line-height: 24px;
      }
    }
  }
  .bulletin-wrapper{
    position: relative;
    padding: 0 22px 0 12px;
    height: 28px;
    line-height: 28px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    background-color: rgba(7,17,27,0.2);
    .bulletin-title{
      display: inline-block;
      vertical-align: top;
      margin-top: 9px;
      width: 22px;
      height: 12px;
      .bg-image('bulletin');
      background-size: 22px 12px;
      background-repeat: np-repeat;
    }
    .bulletin-text{
      vertical-align: top;
      font-size: 10px;
      margin: 0 4px;
    }
    .icon-keyboard_arrow_right{
      font-size: 10px;
      position: absolute;
      right: 12px;
      bottom: 6px;
    }
  }
  .background{
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    filter: blur(10px);
    overflow: hidden;
  }
  .fade-enter-active {
    transition: all .3s ease;
  }
  .fade-leave-active {
    transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .fade-enter, .fade-leave-active {
    transform: translateX(10px);
    opacity: 0;
  }
  .detail{
    position: fixed;
    top: 0;
    left: 0;
    z-index: 51;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(7,17,27,0.8);
    .detail-wrapper{
      min-height: 100%;
      width: 100%;
      .detail-main{
        margin-top: 64px;
        padding-bottom: 64px;
        .name{
          line-height: 16px;
          font-size: 16px;
          text-align: center;
          font-weight: 700;
        }
        .star-wrapper{
          margin-top: 18px;
          padding: 2px 0;
          text-align: center;
        }
        .title{
          width: 80%;
          margin: 28px auto 24px;
          display: flex;
          .line{
            flex: 1;
            position: relative;
            top: -6px;
            border-bottom: 1px solid rgba(255,255,255,0.2);
          }
          .text{
            padding: 0 12px;
            font-size: 14px;
            font-weight: 700px;
          }
        }
        .supports{
          width: 80%;
          margin: 0 auto;
          .support-item{
            padding: 0 12px;
            margin-bottom: 12px;
            font-size: 0;
            &:last-child{
              margin-bottom: 0;
            }
            .icon{
              display: inline-block;
              width: 16px;
              height: 16px;
              vertical-align: top;
              margin-right: 6px;
              background-size: 16px 16px;
              background-repeat: no-repeat;
              &.decrease{
                .bg-image('decrease_2');
              }
              &.discount{
                .bg-image('discount_2');
              }
              &.guarantee{
                .bg-image('guarantee_2');
              }
              &.invoice{
                .bg-image('invoice_2');
              }
              &.special{
                .bg-image('special_2');
              }
            }
            .text{
              font-size: 12px;
              line-height: 16px;
            }
          }
        }
        .bulletin{
          width: 80%;
          margin: 0 auto;
          .content{
            padding: 0 12px;
            line-height: 24px;
            font-size: 12px;
          }
        }
      }
    }
    .detail-close{
      position: relative;
      font-size: 32px;
      width: 32px;
      height: 32px;
      margin: -64px auto 0 auto;
      clear: both;
    }
  }
}
</style>
