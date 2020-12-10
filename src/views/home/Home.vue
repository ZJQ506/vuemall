<template>
  <div id="home">
    <nav-bar class="home-nav"></nav-bar>
    <home-swiper :banners="banners"></home-swiper>
    <recommend-view :recommends="recommends"></recommend-view>
  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar'
  import HomeSwiper from './childComps/HomeSwiper'
  import {getHomeMultidata} from "network/home";
  import RecommendView from './childComps/RecommendView'

  export default {
    name: "Home",
    components: {
      NavBar,
      HomeSwiper,
      RecommendView
    },
    data() {
      return {
        // 轮播图数据
         banners: [],
        recommends: []
      }
    },
    created() {
      //  1.请求多个数据
      getHomeMultidata().then(res => {
        // console.log(res);
        this.banners = res.data.banner.list
        this.recommends = res.data.recommend.list
      })
    }
  }
</script>

<style scoped>
  .home-nav {
    background-color: var(--color-tint);
  }
</style>
