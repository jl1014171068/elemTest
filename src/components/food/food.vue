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
       </div>
       <div class="cartcontrol-wrapper">
          <cartcontrol :food='food'></cartcontrol>
       </div>
       <div transition='fade' @click='addFirst' v-show='!food.count || food.count === 0' class="buy">
           加入购物车
       </div>
    </div> 
  </div>
</template>

<script type='text/ecmascript-6'>
   import BScroll from 'better-scroll';
   import Vue from 'vue';
   import cartcontrol from 'components/cartcontrol/cartcontrol';
   export default {
   	  props: {
   	  	food: {
   	  		type: Object
   	  	}
   	  },
      data () {
         return {
            showFlag: false
         };
      },
      methods: {
        show () {
           this.showFlag = true;
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
         cartcontrol
      }
   };
</script>

<style lang="scss">
    @import 'food.scss';
</style>