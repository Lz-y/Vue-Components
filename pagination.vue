<template>
  <div class="pagination">
    <span class="total">共{{total}}条</span>
    <button type="button" class="prev" @click="prev" v-if="showPrev">
      <span v-if="nextText">{{prevText}}</span>
      <i class="iconfont lg-back" v-else></i>
    </button>
    <ul class="pager">
      <li class="number" :class="[currentPage === item ? 'active': '']"
       v-for="item in pages" :key="item" @click.prevent="selected(item)">{{item}}</li>
    </ul>
    <button type="button" class="next" @click="next" v-if="showNext">
      <span v-if="nextText">{{nextText}}</span>
      <i class="iconfont lg-next" v-else></i>
    </button>
    <div class="jumper">
      <span>跳转到</span>
      <input type="text" v-model.number="page" @keyup.enter="changePage">
      <span>页</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'lg-pagination',
  props: {
    // 总数
    'total': {
      type: Number,
      required: true
    },
    // 当前页数
    'current-page': {
      type: Number,
      default: 1,
      required: true
    },
    // 页面条数
    'page-size': {
      type: Number,
      default: 10
    },
    // 页码按钮的数量
    'pager-count': {
      type: Number,
      default: 7
    },
    // 上一页的文字
    'prev-text': {
      type: String
    },
    // 下一页的文字
    'next-text': {
      type: String
    }
  },
  data () {
    return {
      page: this.currentPage
    }
  },
  methods: {
    // 按钮选中时触发的事件
    selected (val) {
      this.go(val)
    },
    // 输入前往的页数后按enter触发的事件
    changePage () {
      // 当前页大于总页数
      if (this.page > this.pager) {
        this.page = this.pager
      }
      this.go(this.page)
    },
    // 上一页
    prev () {
      if (this.currentPage === 1) {
        return false
      }
      this.go(this.page - 1)
    },
    // 下一页
    next () {
      // 当前页大于等于总页数
      if (this.currentPage >= this.pager) {
        return false
      }
      this.go(this.page + 1)
    },
    go (page) {
      // 与父组件通信，调用时将当前页数传给父组件
      this.$emit('change-page', page)
      this.page = page
    }
  },
  computed: {
    // 计算总页数
    pager () {
      return Math.ceil(this.total / this.pageSize)
    },
    // 计算页码
    pages () {
      let items = []
      if (this.currentPage <= this.pagerCount) {
        // 判断总页数与页码按钮数量的大小
        let val = Math.min(this.pager, this.pagerCount)
        while (val) {
          items.unshift(val--)
        }
      } else {
        let i = this.pagerCount
        // 从哪开始
        let start = this.currentPage - this.pagerCount + 1
        while (i--) {
          items.push(start++)
        }
      }
      return items
    },
    showPrev () {
      return this.page !== 1
    },
    showNext () {
      return this.page !== this.pager
    }
  }
}
</script>

<style scoped>
.pagination{
  white-space: nowrap;
  color: #303133;
  font-weight: 500;
  font-size: 1.3rem;
  text-align: center;
}
.pagination::before{
  display: table;
  content: ""
}
.pagination::after{
  display: table;
  content: ''
}
.total, button[type=button], .pager .number, .jumper{
  vertical-align: top;
  display: inline-block;
  font-size: 1.3rem;
  min-width: 3.5rem;
  height: 2.8rem;
  line-height: 2.8rem;
}
.total{
  margin-right: 0.8rem;
}
.prev, .next{
  background: #fff;
  border: none;
  margin: 0 0.4rem;
  cursor: pointer;
  outline: none;
}
.prev:hover, .next:hover{
  color: #07b6e0;
}
[class*="lg-"]{
  font-size: 1.8rem;
  font-weight: 700;
}
.pager{
  user-select: none;
  list-style: none;
  display: inline-block;
  vertical-align: top;
  font-size: 0;
}
.pager .number{
  padding: 0 0.4rem;
  background: #fff;
  vertical-align: top;
  display: inline-block;
  font-size: 1.3rem;
  min-width: 3.5rem;
  height: 2.8rem;
  line-height: 2.8rem;
  cursor: pointer;
  box-sizing: border-box;
  text-align: center;
  margin: 0 0.4rem;
}
.pager li.active{
  background: #07b6e0;
  color: #fff;
}
.pager li:hover:not(.active){
  background: #07b6e0;
  color: #fff;
}
input[type=text]{
  max-width: 4rem;
  height: 2.5rem;
  margin: 0 0.4rem;
  border-radius: 0.4rem;
  border: none;
  padding: 0 0.4rem;
  outline: none;
  text-align: center;
}
input:focus{
  outline: none;
  border: 1px solid #07b6e0
}
</style>
