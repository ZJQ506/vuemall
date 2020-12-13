<template>
  <div class="back-top">
    <img src="~assets/img/common/top.png" alt="">
    <!--<img src="../../../assets/img/common/top.png" height="86" width="86"/>-->
  </div>
</template>

<script>
  export default {
    name: "BackTop",

  }
</script>

<style scoped>
  .back-top {
    position: fixed;
    right: 10px;
    bottom: 60px;
  }

  .back-top img {
    width: 43px;
    height: 43px;
  }

</style>
