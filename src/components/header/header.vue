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
     <div v-show='detailShow' class="detail" transition="fade">
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
               <ul  v-if="seller.supports" class="supports">
                  <li class="support-item" v-for="item in seller.supports">
               
                    <span class="icon" :class="classMap[seller.supports[$index].type]"></span>
                    <span class="text">{{seller.supports[$index].description}}</span>
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
  </div>
</template>

<script type='text/ecmascript-6'>
  import star from 'components/star/star';

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
      },
      hideDetail (){
         this.detailShow = false; 
      }
    },
    components: {
       star
    }
  };
</script>

<style lang="scss">
   @import 'header.scss'
</style>