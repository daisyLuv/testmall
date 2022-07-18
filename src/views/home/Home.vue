<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <scroll class="content" ref="scroll" :probe-type="3" @scroll="contentScroll" :pull-up-load="true"
      @pillingUp="loadMore">
      <home-swiper :banners="banners" ref="swiper" />
      <recommend-view :recommends="recommends" />
      <feature-view />
      <tab-control class="tab-control" :titles="['流行', '新款', '精选']" @tabClick="tabClick" />
      <goods-list :goods="showGoods" />
    </scroll>
    <!-- 组件不能直接监听，需要加.native 监听组件根元素的原生事件-->
    <back-top @click.native="backClick" v-show="isShowBackTop" />
  </div>
</template>

<script>
import HomeSwiper from './childComps/HomeSwiper'
import RecommendView from "./childComps/RecommendView";
import FeatureView from "./childComps/FeatureView";

import NavBar from "components/common/navbar/NavBar";
import TabControl from "components/content/tabControl/TabControl";
import GoodsList from 'components/content/goods/GoodsList.vue';
import BackTop from 'components/common/backTop/BackTop.vue';

import { getHomeMultidata, getHomeGoods } from "network/home";

import Scroll from 'components/common/scroll/Scroll.vue';

export default {
  name: "Home",
  components: {
    NavBar,
    HomeSwiper,
    RecommendView,
    FeatureView,
    TabControl,
    GoodsList,
    Scroll,
    BackTop
  },
  data() {
    return {
      // 用 result 存储数据
      // result: null
      // result 里面有很多数据，不方便，所以单独
      banners: [],
      recommends: [],
      goods: {
        'pop': { page: 0, list: [] },
        'new': { page: 0, list: [] },
        'sell': { page: 0, list: [] },
      },
      currentType: 'pop',
      isShowBackTop: true
    }
  },
  computed: {
    showGoods() {
      return this.goods[this.currentType].list
    }
  },
  created() {
    // 组件创建完后就调用数据，在此建立生命周期函数
    // 1.请求多个数据
    this.getHomeMultidata()

    // 请求商品数据
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')

  },
  methods: {
    /**
     * 事件监听相关的方法
     */
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
      this.$refs.scroll.scrollTo(0, 0, 500)
    },
    contentScroll(position) {
      // console.log(position);
      this.isShowBackTop = (-position.y) > 1000
    },
    loadMore() {
      this.getHomeGoods(this.currentType)

      // 刷新可滚动区域高度
      // this.$refs.scroll.scroll.refresh()
    },
    /**
       * 网络请求相关的方法
       */
    getHomeMultidata() {
      getHomeMultidata().then(res => {
        // 函数最后返回的是一个 promise，用then得到数据，存储在data 的 result 里面，箭头函数的this指向上一级，this.result指的是组件的data里的result。
        // res={}，不另外存储的话，函数执行完后res会回收，res指向请求回来的数据{}的指针被回收，垃圾回收机制判断{}孤立，然后回收
        // console.log(res);
        // this.result = res;
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      })
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1
      getHomeGoods(type, page).then(res => {
        // console.log(res)
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1

        this.$refs.scroll.finishPullUp()
      })
    }
  }
}
</script>

<style scoped>
#home {
  /* padding-top: 44px; */
  height: 100vh;
  position: relative;
}

.home-nav {
  background-color: var(--color-tint);
  color: #fff;

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

.content {
  /* height: calc(100% - 93px); */
  overflow: hidden;
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
}

/*.content {*/
/*height: calc(100% - 93px);*/
/*overflow: hidden;*/
/*margin-top: 44px;*/
/*}*/
</style>
