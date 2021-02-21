<template>
  <div>
    <AnimationSale v-if="animationName === 'sale'" :hover="hover" />
    <AnimationSearch v-if="animationName === 'search'" :hover="hover" />
  </div>
</template>

<script>
  import AnimationSale from './animations/Sale';
  import AnimationSearch from './animations/Search';

  export default {
    props: {
      animationName: String,
      hover: Boolean
    },
    components: {
      AnimationSale,
      AnimationSearch
    }
  }
</script>

<style lang="scss" scoped>
  svg {
    height: 150px;
  }
</style>