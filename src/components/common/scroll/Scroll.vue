<template>
  <div class="wrapper" ref="wrapper">
    <div class="content div">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'

// 开启对 content 以及 content 子元素 DOM 改变的探测。当插件被使用后，当这些 DOM 元素发生变化时，将会触发 scroll 的 refresh 方法。 observe-dom 插件具有以下几个特性：
//  1.针对改变频繁的 CSS 属性，增加 debounce
//  2.如果改变发生在 scroll 动画过程中，则不会触发 refresh
// 总结：就是说 observe-dom 这个属性会在 better-scroll 内部帮我们监听 content 的高度变化，自动的帮我们调用内部的 refresh，再也不用为了图片内容早better-scroll对象创建的时候没加载出来导致不能滚动而到处的refresh了。
// 安装：npm install @better-scroll/observe-dom --save
import ObserveDOM from '@better-scroll/observe-dom'

export default {
  name: 'Scroll',
  props: {
    probeType: {
      type: Number,
      default: 0
    },
    pullUpLoad: {
      type: Boolean,
      default: false
    }
  },
  components: {

  },
  data() {
    return {
      scroll: null
    }
  },
  mounted() {
    // document.querySelector 拿 wrapper 不能明确指定是否是想要的，可以直接绑定ref 明确拿到该子组件
    // this.scroll = new BScroll(document.querySelector('.wrapper'))
    // 1.创建BScroll对象
    BScroll.use(ObserveDOM)
    this.scroll = new BScroll(this.$refs.wrapper, {
      click: true,
      // probe: 侦探
      probeType: this.probeType,
      pullUpLoad: this.pullUpLoad,
      // 使用插件，解决图片加载异步 上拉区域高度变化的问题
      observeDOM: true
    })

    // this.scroll.scrollTo(0, 0)

    // 2.监听滚动的位置
    this.scroll.on('scroll', (position) => {
      // console.log(position);
      // position 需要传出去
      this.$emit('scroll', position)
    })

    // 3.监听上拉事件
    this.scroll.on('pullingUp', () => {
      // console.log('上拉')
      this.$emit('pillingUp')
    })
  },
  methods: {
    // es6 默认值，如果time没有传参数 则使用默认值 300
    scrollTo(x, y, time = 300) {
      this.scroll.scrollTo(x, y, time)
    },
    finishPullUp() {
      this.scroll.finishPullUp()
    }
  },
  created() {

  }
}
</script>

<style scoped>
</style>