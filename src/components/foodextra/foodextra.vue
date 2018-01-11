<template>
  <div class="extra">
    <span class="count">月售{{food.sellCount}}</span><span>好评率{{food.rating}}%</span>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
        food: {
            type: Object
        }
    }
  };
</script>

<style lang="less">
    .extra {
        font-size: 10px;
        color: rgb(147, 153, 159);
        .count {
            margin-right: 12px;
        }
    }
</style>