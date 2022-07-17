<template>
  <div class="wrapper" ref="aaaa">
    <div class="content">
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
      <h2>分类</h2>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'

export default {
  name: 'Category',
  components: {

  },
  data() {
    return {
      scroll: null
    }
  },
  methods: {

  },
  // created() {
  //   new BScroll('.wrapper', {

  //   })
  // },
  mounted() {
    this.scroll = new BScroll(this.$refs.aaaa, {
    
    })
  }
}
</script>

<style scoped>
.wrapper {
  height: 300px;
  background-color: red;
  overflow: hidden;
}
</style>