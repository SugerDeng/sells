<template>
  <div class="star" :class="starType">
    <span v-for="(itemClass, index) in itemClasses" :key="index" :class="itemClass" class="starItem"></span>
  </div>
</template>

<script type="text/ecmascript-6">
  const LENGTH = 5;
  const CLS_ON = "on";
  const CLS_HALF = "half";
  const CLS_OFF = "off";

  export default {
    props: {
        size: {
            type: Number
        },
        score: {
            type: Number
        }
    },
    computed: {
        starType() {
            return 'star-' + this.size;
        },
        itemClasses() {
            let result = [];
            let score = Math.floor(this.score * 2) / 2;
            let hasDecimal = score % 1 !==0;
            let integer = Math.floor(score);
            for (let i = 0; i < integer; i ++) {
                result.push(CLS_ON);
            }
            if (hasDecimal) {
                result.push(CLS_HALF);
            }
            while (result.length < LENGTH) {
                result.push(CLS_OFF);
            }
            return result;
        }
    }
  }
</script>

<style lang="less">
    @import '../../common/less/mixin.less';

    .star {
        font-size: 0;
        .star-item {
            display: inline-block;
            background-repeat: no-repeat;
        }
        &.star-48 {
            .star-item {
                width: 20px;
                height: 20px;
                margin-right: 22px;
                background-size: 20px 20px;
                &:last-child {
                    margin-right: 0;
                }
                &.on {
                    .bg-image('start48_on');
                }
            }
        }
        &.star-36 {
            
        }
        &.star-24 {
            
        }
    }
 
</style>