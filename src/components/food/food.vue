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
            <split></split>
            <div class="rating">
                <h1 class="title">商品评价</h1>
                <ratingselect @ratingTypeSelect="ratingTypeSelect" @contentToggle="contentToggle" :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
                <div class="rating-wrapper">
                    <ul v-show="food.ratings && food.ratings.length">
                        <li v-show="needShow(rating.rateType, rating.text)" v-for="(rating, index) in food.ratings" :key="index" class="rating-item border-1px">
                            <div class="user">
                                <span class="name">{{rating.username}}</span>
                                <img class="avatar" :src="rating.avatar" width="12px" height="12px">
                            </div>
                            <div class="time">{{rating.rateTime | formatDate}}</div>
                            <p class="text">
                                <span :class="{'icon-thumb_up': rating.rateType === 0, 'icon-thumb_down': rating.rateType === 1}"></span>{{rating.text}}
                            </p>
                        </li>
                    </ul>
                    <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
                </div>
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
  import {formatDate} from '../../common/js/date';
  import split from '../split/split.vue';
  import ratingselect from '../ratingselect/ratingselect.vue';

  const POSITIVE = 0;
  const NEGATIVE = 1;
  const ALL = 2;

  export default {
    props: {
        food: {
            type: Object
        }
    },
    data() {
      return {
          showFlag: false,
          selectType: ALL,
          onlyContent: false,
          desc: {
              all: '全部',
              positive: '推荐',
              negative: '吐槽'
          }
      }
    },
    filters: {
        formatDate(time) {
            let date = new Date(time);
            return formatDate(date, 'yyyy-MM-dd hh:mm');
        }
    },
    methods: {
        show() {
            this.showFlag = true;
            this.selectType = ALL;
            this.onlyContent = false;
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
        },
        needShow(type, text) {
            if (this.onlyContent && !text) {
                return false;
            }
            if (this.selectType === ALL) {
                return true;
            } else {
                return type == this.selectType;
            }
        },
        ratingTypeSelect(type) {
            this.selectType = type;
        },
        contentToggle(onlyContent) {
            this.onlyContent = onlyContent;
        }
    },
    components: {
        price,
        foodextra,
        cartcontrol,
        split,
        ratingselect
    }
  };
</script>

<style lang="less">
    @import '../../common/less/mixin.less';

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
        .rating {
            padding-top: 18px;
            .title {
                margin-left: 18px;
                font-size: 14px;
                color: rgb(7, 17, 27);
                line-height: 14px;
            }
            .rating-wrapper {
                padding: 0 18px;
                .rating-item {
                    position: relative;
                    padding: 16px 0;
                    .border-1px(rgba(7, 17, 27, 0.1));
                    .user {
                        position: absolute;
                        right: 0;
                        top: 16px;
                        line-height: 12px;
                        font-size: 0;
                        .name {
                            display: inline-block;
                            margin-right: 6px;
                            vertical-align: top;
                            font-size: 10px;
                            color: rgb(147, 153, 159);
                        }
                        .avatar {
                            border-radius: 50%;
                        }
                    }
                    .time {
                        margin-bottom: 6px;
                        line-height: 12px;
                        font-size: 10px;
                        color: rgb(147, 153, 159);
                    }
                    .text {
                        font-size: 12px;
                        color: rgb(7, 17, 27);
                        line-height: 16px;
                        .icon-thumb_up, .icon-thumb_down {
                            margin-right: 4px;
                            font-size: 12px;
                            line-height: 16px;
                        }
                        .icon-thumb_up {
                            color: rgb(0, 160, 220);
                        }
                        .icon-thumb_down {
                            color: rgb(147, 153, 159);
                        }
                    }
                }
                .no-rating {
                    padding: 16px 0;
                    font-size: 12px;
                    color: rgb(147, 153, 159);
                }
            }
        }
    }
</style>