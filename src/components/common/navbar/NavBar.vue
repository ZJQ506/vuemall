<template>
  <div class="nav-bar">
    <div class="left"><slot name="left"></slot></div>
    <div class="center"><slot name="center">生机商城</slot></div>
    <div class="right"><slot name="right"></slot></div>
  </div>
</template>

<script>
  export default {
    name: "NavBar"
  }
</script>

<style scoped>
   .nav-bar{
     display: flex;
     line-height: 44px ;
     height: 44px;
     text-align: center;
     color: white;
       box-shadow: 0 1px 1px rgba(150,150,150,.08);

   }
  .left,.right{
    width: 60px;
   }
  .center{
    flex: 1;
    /*color: white;*/

  }
</style>
