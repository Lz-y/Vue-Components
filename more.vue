<template>
  <div class="more" :style="{width: width}" @click.prevent="loadMore">
    <div class="container">
      <span>{{content}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'lg-more',
  props: {
    content: {
      type: String,
      default: '加载更多'
    },
    width: {
      type: String,
      default: '100px'
    }
  },
  methods: {
    loadMore () {
      this.$emit('loadMore')
    }
  }
}
</script>

<style scoped>
.more {
  text-align: center;
  height: 3.5rem;
  line-height: 3.5rem;
  background: rgba(87,87,87,0.5);
  color: #fff;
  border-radius: 3rem;
  font-size: 1.4rem;
  padding: 0 1rem;
  display: inline-block;
  cursor: pointer;
  -webkit-transition: all 0.5s;
  -moz-transition: all 0.5s;
  -o-transition: all 0.5s;
  transition: all 0.5s;
}
.container span{
  position: relative;
  display: inline-block;
  -webkit-transition: 0.5s;
  -moz-transition: 0.5s;
  -o-transition: 0.5s;
  transition: 0.5s;
}
.container:hover span{
  padding-right: 2rem;
}
.container:hover span::after{
  opacity: 1;
  right: 0;
}
span::after{
  content: '>>';
  position: absolute;
  top: 0;
  right: -2.5rem;
  opacity: 0;
  color: #fff;
  font-size: 1.3rem;
  -webkit-transition: 0.5s;
  -moz-transition: 0.5s;
  -o-transition: 0.5s;
  transition: 0.5s;
}
</style>
