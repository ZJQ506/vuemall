<template>
  <div id="home">
    <nav-bar class="home-nav"></nav-bar>

    <scroll class="content" ref="scroll" :probe-type="3"><!--滚动插件-->
      <home-swiper :banners="banners"></home-swiper><!--轮播图-->
      <recommend-view :recommends="recommends"></recommend-view><!--四个圆图-->
      <feature-view></feature-view><!--滚动插件-->
      <!--滚动插件-->
      <tab-control :titless="['流行','新款','精选']"
                   @tabClick1="tabClick"
                   class="tab-control"></tab-control>
      <goods-list :goods="showGoods"></goods-list>
    </scroll>
    <!--native 监听组件的根元素的原生事件-->
    <back-top @click.native="backClick"></back-top><!--返回顶部插件-->
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
  import Scroll from 'components/common/scroll/Scroll'
  import BackTop from 'components/content/backTop/BackTop'

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
      GoodsListItem,
      Scroll,
      BackTop
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
        currentType: 'pop'

      }
    },
    computed: {
      showGoods() {
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
      tabClick(index) {
        switch (index) {
          case 0:
            this.currentType = 'pop'
            break
          case 1:
            this.currentType = 'new'
            break
          case 2:
            this.currentType = 'sell'
            break
        }
      },

      backClick() {
        // this.$ref.scroll.scrollTo()
        // 没有用scroll的返回顶部
        window.scrollTo({top:0,behavior:"smooth"});
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
  /*.content{*/
    /*height: 99vh;*/
  /*}*/

</style>
