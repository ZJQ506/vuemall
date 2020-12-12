<template>
  <div id="home">
    <nav-bar class="home-nav"></nav-bar>
    <home-swiper :banners="banners"></home-swiper>
    <recommend-view :recommends="recommends"></recommend-view>
    <feature-view></feature-view>
    <tab-control :titless="['流行','新款','精选']"
                 @tabClick1="tabClick"
                 class="tab-control"></tab-control>
    <goods-list :goods="showGoods"></goods-list>
   </div>
</template>

<script>
  import HomeSwiper from './childComps/HomeSwiper'
  import RecommendView from './childComps/RecommendView'
  import FeatureView from './childComps/FeatureView'

  import NavBar from 'components/common/navbar/NavBar'
  import TabControl from 'components/content/tabControl/TabControl'
  import GoodsList from 'components/content/goods/GoodsList'
  import GoodsListItem from 'components/content/goods/GoodsListItem'


  import {getHomeMultidata, getHomeGoods} from "network/home"


  export default {
    name: "Home",
    components: {
      HomeSwiper,
      RecommendView,
      FeatureView,

      NavBar,
      TabControl,
      GoodsList,
      GoodsListItem
    },
    data() {
      return {
        // 轮播图数据
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []},
        },
        //默认当前类型
        currentType:'pop'

      }
    },
    computed:{
      showGoods(){
        return this.goods[this.currentType].list
      }
    },
    created() {
      //  1.请求多个数据
      this.getHomeMultidata()
      //  2.请求商品数据
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')
    },
    methods: {
      /**
       * 网络请求相关方法
       * */
      //轮播图
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          this.banners = res.data.banner.list
          this.recommends = res.data.recommend.list
        })
      },
      // 两列图片数据
      getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          console.log(res);
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1
        })
      },
      /**
       * 事件监听相关方法
       * */
    //  导航点击切换获取数据
      tabClick(index){
         switch (index) {
           case 0:
             this.currentType='pop'
             break
           case 1:
             this.currentType='new'
             break
           case 2:
             this.currentType='sell'
             break
         }
      }

    }

  }
</script>

<style scoped>
  #home {
    padding-top: 44px;
  }

  .home-nav {
    background-color: var(--color-tint);
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
  }

  .tab-control {
    position: sticky;
    top: 44px;
    z-index: 9;
  }
</style>
