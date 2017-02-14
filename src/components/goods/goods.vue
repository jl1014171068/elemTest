<template>
  <div class="goods">
     <div class="menu-wrapper" v-el:menu-wrapper>
     	<ul>
     		<li @click="selectMenu($index,$event)" class="menu-item" v-for="item in goods" :class="{'current':currentIndex===$index}">
     			<span class="text"><span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}</span>
     		</li>
     	</ul>
     </div>
     <div class="foods-wrapper" v-el:foods-wrapper>
     	<ul>
     		<li v-for="item in goods" class="food-list food-list-hook">
     			<h1 class="title">{{item.name}}</h1>
     			<ul>
     				<li v-for="food in item.foods" class="food-item">
     					<div class="icon">
     						<img width="57px" :src="food.icon">
     					</div>
     					<div class="content">
     						<h2 class="name">{{food.name}}</h2>
     						<p class="desc">{{food.description}}</p>
     						<div class="extra">
     							<span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
     						</div>
     						<div class="price">
     							<span class="now">￥{{food.price}}</span><span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
     						</div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
     					</div>
     				</li>
     			</ul>
     		</li>
     	</ul>
     </div>
    <shopCart :delivery-price="seller.deliveryPrice"
                :min-price="seller.minPrice" ></shopCart>
  </div>
</template>

<script type='text/ecmascript-6'>
  import BScroll from 'better-scroll';
  import shopcart from 'components/shopcart/shopcart';
  import cartcontrol from 'components/cartcontrol/cartcontrol';
  const ERR_OK = 0;

  export default {
  	props: {
      seller: {
        type: Object
      }
    },
  	data () {
  		return {
  			goods: [],
  			listHeight: [],
  			scrollY: 0
  		};
  	},
  	computed: {
       currentIndex () {
            for (let i = 0; i < this.listHeight.length; i++){
            let height1 = this.listHeight[i];
            let height2 = this.listHeight[i + 1];
            if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          	    return i;
          	}
          }
          return 0;
       }
  	},
  	created () {
  	this.$http.get('/api/goods').then((response) => {
            response = response.body;
            if (response.errno === ERR_OK) {
              this.goods = response.data;
              this.$nextTick(() => {
                this._initScroll();
                this._calculateHeight();
              });
            }
  	});
  	this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
  methods: {
  	_initScroll (){
  		this.menuScroll = new BScroll(this.$els.menuWrapper, {
  			click: true
  		});
  		this.foodsScroll = new BScroll(this.$els.foodsWrapper, {
  			probeType: 3
  		});

  		this.foodsScroll.on('scroll', (pos) => {
            this.scrollY = Math.abs(Math.round(pos.y));
  		});
  	},
  	_calculateHeight (){
        let foodList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook');  
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
  	},
  	selectMenu (index, event) {
  	  if (!event._constructed) {
        return;
  	  }
  	  let foodList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook');
  	  let el = foodList[index];
  	  this.foodsScroll.scrollToElement(el, 300);
  	}
  },
  components: {
    shopcart,
    cartcontrol
  }
 };
</script>

<style lang="scss">
    @import 'goods.scss'
</style>