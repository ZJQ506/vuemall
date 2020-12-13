<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    name: "Scroll",
    props:{
      probeType:{
        type:Number,
        default:0
      }
    },
    data() {
      return {
        scroll: null
      }
    },
    // mounted() {
    updated() {
      this.scroll = new BScroll(this.$refs.wrapper,
        {
          click: true,
          probeType: this.probeType
        })
    },
    methods: {
      scrollTo(x, y, time = 300) {
        this.scroll.scrollTo(x, y, time)
      }
    }
  }
</script>

<style scoped>
  .wrapper {
    observeDOM: true;

  }
</style>
