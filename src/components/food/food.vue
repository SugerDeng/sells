<template>
  <transition name="move">
    <div v-show="showFlag" class="food" ref="food">
        <div class="food-content">
            <div class="image-header">
                <img :src="food.image">
                <div class="back" @click="back">
                    <i class="icon-arrow_lift"></i>
                </div> 
            </div>
            <div class="content">
                <h1 class="title">{{food.name}}</h1>
                <foodextra :food="food"></foodextra>
                <price :food="food"></price>
                <div class="cartcontrol-wrapper">
                    <cartcontrol @cartAdd="_drop" :food="food"></cartcontrol>
                </div>
                <transition name="fade">
                    <div class="buy" v-show="!food.count || food.count === 0" @click="addFirst">
                        加入购物车
                    </div>
                </transition>
            </div>    
            <split v-show="food.info"></split>
            <div class="info" v-show="food.info">
                <h1 class="title">商品信息</h1>
                <p class="text">{{food.info}}</p>
            </div>
        </div>
    </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import price from '../price/price.vue';
  import foodextra from '../foodextra/foodextra.vue';
  import BScroll from 'better-scroll';
  import cartcontrol from '../cartcontrol/cartcontrol.vue';
  import Vue from 'vue';
  import split from '../split/split.vue';

  export default {
    props: {
        food: {
            type: Object
        }
    },
    data() {
      return {
          showFlag: false
      }
    },
    computed: {
        
    },
    methods: {
        show() {
            this.showFlag = true;
            this.$nextTick(() => {
                if (!this.scroll) {
                    this.scroll = new BScroll(this.$refs.food, {
                        click: true
                    });
                }
            });
        },
        back() {
            this.showFlag = false;
        },
        addFirst() {
            this._drop();
            Vue.set(this.food, 'count', 1);
        },
        _drop() {
            this.$emit('cartAdd', event.target);
        }
    },
    components: {
        price,
        foodextra,
        cartcontrol,
        split
    }
  };
</script>

<style lang="less">
    .food {
        position: fixed;
        left: 0;
        top: 0;
        bottom: 48px;
        z-index: 30;
        width: 100%;
        background: #fff;
        transition: all 0.2s linear;
        transform: translate3d(0, 0, 0);
        &.move-enter, &.move-leave-to {
            transform: translate3d(100%, 0, 0);
        }
        .image-header {
            position: relative;
            width: 100%;
            height: 0;
            padding-top: 100%;
            img {
                position: absolute;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
            }
            .back {
                position: absolute;
                top: 5px;
                left: 0;
                .icon-arrow_lift {
                    display: block;
                    padding: 10px;
                    font-size: 20px;
                    color: #fff;
                }
            }
        }
        .content {
            position: relative;
            padding: 18px;
            .title {
                line-height: 14px;
                margin-bottom: 8px;
                font-size: 14px;
                font-weight: 700;
                color: rgb(7, 17, 27);
            }
            .extra {
                margin-bottom: 18px;
            }
            .cartcontrol-wrapper {
                position: absolute;
                right: 12px;
                bottom: 12px;
            }
            .buy {
                position: absolute;
                right: 18px;
                bottom: 18px;
                z-index: 10;
                height: 24px;
                line-height: 24px;
                padding: 0 12px;
                box-sizing: border-box;
                font-size: 10px;
                color: #fff;
                border-radius: 12px;
                background: rgb(0, 160, 220);
                transition: all 0.2s;
                &.fade-enter, &.fade-leave-to {
                    opacity: 0;
                }
            }
        }
        .info {
            padding: 18px;
            .title {
                font-size: 14px;
                margin-bottom: 6px;
                color: rgb(7, 17, 27);
                line-height: 14px;
            }
            .text {
                line-height: 24px;
                font-size: 12px;
                padding: 0 8px;
                color: rgb(77, 85, 93);
                
            }
        }
    }
</style>