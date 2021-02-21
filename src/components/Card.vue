<template>
  <div class="card" @mouseover="hover = true" @mouseleave="hover = false">
    <Animation :animationName="animationName" :hover="hover" />
    <h2 class="title">{{ title }}</h2>
  </div>
</template>

<script>
  import Animation from './Animation';

  export default {
    props: {
      title: String,
      animationName: String
    },
    components: {
      Animation
    },
    data () {
      return {
        hover: false
      }
    }
  }
</script>

<style lang="scss" scoped>
  .card {
    box-shadow: 0 0 4px rgba(0,0,0,0.25);
    background: var(--white);
    padding: 1.25rem;
    cursor: pointer;
    display: grid;
    align-items: center;
    justify-content: center;
    text-align: center;
    transition: ease all .2s;
    border-radius: 10px;

    &:hover,
    &:focus {
      box-shadow: 0 0 16px rgba(0,0,0,0.35);
    }

    .title {
      margin: 0;

      &:after{
         content: '.';
         color: var(--pink);
      }
    }
  }
</style>