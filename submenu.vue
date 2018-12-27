<template>
  <li class="lg-submenu">
    <div class="lg-submenu__title" @click="open">
      <i class="iconfont" :class="data.icon"></i>
      <span class="title">{{data.title}}</span>
      <i class="iconfont lg-down" :class="[isOpen ? 'rotate' : '' ]"></i>
    </div>
    <ul class="lg-menu" :style="{display: isOpen ? 'block': 'none'}">
      <template v-for="(item,index) in data.children">
        <li class="lg-menu-item" :class="[$route.name === item.path.name ? 'active' : '']" :key="index">
          <router-link :to="item.path">{{item.name}}</router-link>
        </li>
      </template>
    </ul>
  </li>
</template>

<script>
export default {
  name: 'lg-submenu',
  props: {
    data: {
      type: Object,
      default: function () {
        return {
          title: '',
          icon: '',
          children: []
        }
      },
      required: true
    }
  },
  data () {
    return {
      isOpen: false
    }
  },
  methods: {
    open () {
      this.isOpen = !this.isOpen
    }
  },
  created () {
    this.isOpen = this.data.children.some(item => item.path.name === this.$route.name)
  },
  beforeDestroy () {
    this.isOpen = this.data.children.some(item => item.path.name === this.$route.name)
  }
}
</script>

<style scoped>
li{
  list-style: none;
  font-size: 14px;
}
.lg-submenu__title:hover, .lg-menu-item:hover{
  background: #505050;
  color: #d6d6d6;
}
.lg-submenu__title{
  height: 56px;
  line-height: 56px;
  text-decoration: none;
  color: #c5c5c5;
  position: relative;
  display: inline-block;
  width: 100%;
  box-sizing: border-box;
  cursor: pointer;
  padding: 0 20px;
  border-bottom: 1px solid #504f4f;
}
.title{
  margin-left: 10px;
}
.iconfont{
  font-size: 25px;
  vertical-align: middle;
  text-align: center;
}
.lg-down {
  position: absolute;
  right: 15px;
  top: 0;
  font-size: 14px;
  transition: transform 0.3s;
}
.rotate{
  transform: rotate(180deg);
}
li.lg-menu-item {
  height: 50px;
  line-height: 50px;
  padding: 0 45px;
  cursor: pointer;
  box-sizing: border-box;
  /* border-bottom: 1px solid #504f4f; */
}
.active{
  background: #505050;
  color: #d6d6d6 !important;
}
a {
  color: #c5c5c5;;
  text-decoration: none;
  display: inline-block;
  width: 100%;
}
.router-link-exact-active {
  color: #d6d6d6 !important;
}
</style>
