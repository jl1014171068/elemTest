<template>
  <div class="food" v-show='showFlag' transition='move' v-el:food>	 
    <div class="food-content">
     <div class="image-header">
       <img :src='food.image'>
       <div class="back" @click='hide'>
        <i class="icon-arrow_lift"></i>
      </div>
    </div>
    <div class="content">
      <h1 class="title">{{food.name}}</h1>
      <div class="detail">
        <span class="sell-count">月售{{food.sellCount}}份</span>
        <span class="rating">好评率{{food.rating}}%</span>
      </div>

      <div class="price">
        <span class="now">￥ {{food.price}}</span><span class="old" v-show='food.oldPrice'>￥ {{food.oldPrice}}</span>
      </div>

      <div class="cartcontrol-wrapper">
        <cartcontrol :food='food'></cartcontrol>
      </div>

      <div transition='fade' @click='addFirst' v-show='!food.count || food.count === 0' class="buy">
       加入购物车
     </div>
   </div>
   <split v-show='food.info'></split>
   <div class="info" v-show='food.info'>
     <h1 class="title">商品信息</h1>
     <p class="text">{{food.info}}</p>
   </div>
   <split ></split>
   <div class="rating">
     <h1 class="title">商品评价</h1>
     <ratingselect :ratings='food.ratings' :select-type='selectType' :only-content='onlyContent' :desc='desc'></ratingselect>
     <div class="rating-wrapper">
        <ul v-show='food.ratings&&food.ratings.length'>
          <li v-for='rating in food.ratings' class='rating-item'>
            <div class="user">
              <span class="name">{{rating.username}}</span>
              <img class="avatar" width='12' height='12' :src='rating.avatar' />
            </div>
            <div class="time">{{rating.rateTime}}</div>
            <p class="text">
              <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>{{rating.text}}
            </p>
          </li>
        </ul>
        <div class="no-rating" v-show='!food.ratings || !food.ratings.length'></div>
     </div>
   </div>
 </div> 
</div>
</template>

<script type='text/ecmascript-6'>
   import BScroll from 'better-scroll';
   import Vue from 'vue';
   import cartcontrol from 'components/cartcontrol/cartcontrol';
   import split from 'components/split/split';
   import ratingselect from 'components/ratingselect/ratingselect';

  // const POSITIVE = 0;
  // const NEGATIVE = 1;
  const ALL = 2;
   export default {
   	  props: {
   	  	food: {
   	  		type: Object
   	  	}
   	  },
      data () {
         return {
            showFlag: false,
            selectType: ALL,
            onlyContent: true,
            desc: {
              all: '全部',
              positive: '推荐',
              negative: '吐槽'
            }
         };
      },
      methods: {
        show () {
           this.showFlag = true;
           this.selectType = ALL;
           this.onlyContent = true;
           this.$nextTick(() => {
              if (!this.scroll) {
                this.scroll = new BScroll(this.$els.food, {
                  click: true
                });
              } else {
                this.scroll.refresh();
              }
           });
        },
        hide () {
           this.showFlag = false;
        },
        addFirst (event) {
           if (!event._constructed) {
            return;
          }
          this.$dispatch('cart.add', event.target);
           Vue.set(this.food, 'count', 1);
        }
      },
      components: {
         cartcontrol,
         split,
         ratingselect
      }
   };
</script>

<style lang="scss">
    @import 'food.scss';
</style>