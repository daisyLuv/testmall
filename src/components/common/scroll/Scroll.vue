<template>
  <div class="wrapper" ref="wrapper">
    <div class="content div">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

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
      this.scroll = new BScroll(this.$refs.wrapper, {
        click: true,
        // probe: 侦探
        probeType: this.probeType,
        pullUpLoad: this.pullUpLoad
      })

      this.scroll.scrollTo(0, 0)
      
      // 2.监听滚动的位置
    },
    methods:{
      // es6 默认值，如果time没有传参数 则使用默认值 300
      scrollTo(x, y, time=300) {
        this.scroll.scrollTo(x, y, time)
      }
    },
    created(){
      
    }
  }
</script>

<style scoped>
  
</style>