<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <home-swiper :banners="banners"/>
    <recommend-view :recommends="recommends"/>
  </div>
</template>

<script>
import NavBar from "components/common/navbar/NavBar";
import HomeSwiper from './childComps/HomeSwiper'
import RecommendView from "./childComps/RecommendView";

import { getHomeMultidata } from "network/home";


export default {
  name: "Home",
  components: {
    NavBar,
    HomeSwiper,
    RecommendView
},
  data() {
    return {
      // 用 result 存储数据
      // result: null
      // result 里面有很多数据，不方便，所以单独
      banners: [],
      recommends: []
    }
  },
  created() {
    // 组件创建完后就调用数据，在此建立生命周期函数
    // 1.请求多个数据
    getHomeMultidata().then(res => {
      // 函数最后返回的是一个 promise，用then得到数据，存储在data 的 result 里面，箭头函数的this指向上一级，this.result指的是组件的data里的result。
      // res={}，不另外存储的话，函数执行完后res会回收，res指向请求回来的数据{}的指针被回收，垃圾回收机制判断{}孤立，然后回收
      // console.log(res);
      // this.result = res;
      this.banners = res.data.banner.list;
      this.recommends = res.data.recommend.list;
    })
  }
}
</script>

<style scoped>
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
}
</style>
