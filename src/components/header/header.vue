<template>
  <div class="v-header">
     <div class="content-wrapper">
        <div class="avatar">
          <img width="64" height='64' :src="seller.avatar" alt="">
        </div>
        <div class="content">
           <div class="title">
             <span class="brand"></span>
             <span class="name">{{seller.name}}</span>
           </div>
           <div class="description">
             {{seller.description}}/{{seller.deliveryTime}}分钟送达
           </div>
           <div v-if='seller.supports' class='supports'>
             <span class="icon" :class='classMap[seller.supports[0].type]'></span>
             <span class="text">{{seller.supports[0].description}}</span>
           </div>
           <div @click='showDetail' v-if='seller.supports' class="supports-count">
              <span class="count">{{seller.supports.length}}个</span>
              <i class="icon-keyboard_arrow_right"></i>
           </div>
        </div>
     </div>
     <div @click='showDetail' class="bulletin-wrapper">
       <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
       <i class="icon-keyboard_arrow_right"></i>
     </div>
     <div class="background">
       <img width='100%' height="100%" :src="seller.avatar" alt="">
     </div>
     <div v-show='detailShow' class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
          </div>
        </div>
        <div class="detail-close">
          <i class="icon-close"></i>
        </div>
     </div>
  </div>
</template>

<script type='text/ecmascript-6'>
  export default {
     props: {
       seller: {
         type: Object
       }
     },
     data () {
       return {
          detailShow: false
       };
     },
     created () {
       this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    methods: {
      showDetail () {
         this.detailShow = true;
      }
    }
  };
</script>

<style lang="scss">
@import '../../common/style/mixin.scss';
      .v-header{
        position:relative;
        color:#fff;
        overflow:hidden;
        background: rgba(7,17,27,.5);
        .content-wrapper{
         position:relative;
         padding:24px 12px 18px 24px;
         font-size:0;
         .avatar{
          display:inline-block;
          vertical-align: top;
          img{
            border-radius:2px;
          }
        }
        .content{
          display:inline-block;
          margin-left:16px;
          .title{
            margin:2px 0 8px 0;
            .brand{
              width:30px;
              height:18px;
              display:inline-block;
              @include bg-img('brand');
              vertical-align: top;
              background-size:30px 18px;
              background-repeat:no-repeat;
            }
            .name{
              margin-left:6px;
              font-size:16px;
              line-height:18px;
              font-weight:bold;
            }
          }
          .description{
            margin-bottom:10px;
            line-height:12px;
            font-size:12px;
          }
          .supports{
            .icon{
             display:inline-block;
             vertical-align: top;
             width:12px;
             height:12px;
             margin-right:4px;
             background-size:12px 12px;
             background-repeat:no-repeat;
             &.decrease{
               @include bg-img('decrease_1');
             }
             &.discount{
               @include bg-img('discount_1');
             }
             &.guarantee{
               @include bg-img('guarantee_1');
             }
             &.invoice{
               @include bg-img('invoice_1');
             }
             &.special{
               @include bg-img('special_1');
             }
           }
           .text{
             display:inline-block;
             line-height:12px;
             font-size:10px;
             color:#fff;
           }
         }
         .supports-count{
           position:absolute;
           right:12px;
           bottom:14px;
           padding:0 8px;
           height:24px;
           line-height:24px;
           border-radius:14px;
           background:rgba(0,0,0,.2);
           text-align:center;
           .count{
            vertical-align: top;
            font-size:10px;
          }
          i{
            font-size:10px;
            line-height:24px;
            margin-left:2px;
          }
        }
      }
      }
      .bulletin-wrapper{
        position:relative;
        height:28px;
        line-height:28px;
        padding:0 22px 0 12px;
        white-space:nowrap;
        overflow:hidden;
        text-overflow:ellipsis;
        background:rgba(7,17,27,.2);
        .bulletin-title{
         display:inline-block;
         margin-top:8px;
         vertical-align: top;
         width:22px;
         height:12px;
         @include bg-img('bulletin');
         background-size:22px 12px;
         background-repeat:no-repeat;
       }
       .bulletin-text{
        vertical-align: top;
        margin:0 4px;
        font-size:10px;
              // display:inline-block;
            }
            i{
              position:absolute;
              font-size:10px;
              right:12px;
              top:8px;
            }
          }
          .background{
            position: absolute;
            top:0;
            left:0;
            width:100%;
            height:100%;
            z-index:-1;
            filter:blur(10px);
          }
          .detail{
           position:fixed;
           top:0;
           left:0;
           z-index:100;
           width:100%;
           height:100%;
           overflow:auto;
           background:rgba(7,17,27,.8);
           .detail-wrapper{
             min-height:100%;
             .detail-main{
              margin-top:64px;
              padding-bottom:64px;
            }
          }
          .detail-close{
            position: relative;
            width:32px;
            height:32px;
            margin:-64px auto 0 auto;
            clear:both;
            font-size:32px;
          }
        }
      }
  // 顶部结束
</style>