<template>
  <div class="lg-select">
    <div class="lg-input" :class="{'is-focus': isFocus}">
      <span class="lg-input__prefix">
        <i class="iconfont" :class="icon"></i>
      </span>
      <input type="text" ref="select" class="lg-input__inner" :value="model" :placeholder="placeholder"
      readonly @click="select">
      <span class="lg-input__suffix">
        <i class="iconfont lg-down"></i>
      </span>
    </div>
    <div class="lg-select-dropdown">
      <div class="upper"></div>
      <div class="lg-dropdown">
        <ul class="lg-dropdown-list">
          <li class="lg-dropdown-item" v-for="(item,index) in options" :key="index" @click="pick(item)">
            <span>{{item}}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'lg-select',
  model: {
    prop: 'model',
    event: 'change'
  },
  props: {
    icon: [String],
    placeholder: {
      type: String,
      default: '请选择'
    },
    model: {
      type: [Number, String],
      required: true
    },
    options: {
      type: Array,
      required: true
    }
  },
  data () {
    return {
      isFocus: false
    }
  },
  methods: {
    select () {
      if (this.isFocus) {
        this.$emit('change', this.$refs.select.value)
      }
      this.isFocus = !this.isFocus
    },
    pick (val) {
      this.isFocus = false
      this.$emit('change', val)
    }
  }
}
</script>

<style scoped>
.lg-select{
  position: relative;
  width: 300px;
  box-sizing: border-box;
}
i[class*='lg-']{
  width: 30px;
  height: 36px;
  line-height: 36px;
  display: inline-block;
  text-align: center;
  vertical-align: middle;
}
.lg-input__inner{
  border: 1px solid #ddd;
  height: 36px;
  line-height: 36px;
  width: 100%;
  padding: 0 30px 0 10px;
  display: inline-block;
  box-sizing: border-box;
  border-radius: 4px;
  cursor: pointer;
}
input:focus{
  border: 1px solid #07b6e0;
  outline: none;
}
.lg-input__prefix, .lg-input__suffix{
  position: absolute;
  color: #9e9191;
}
.lg-input__suffix{
  right: 0;
  top: 0;
}
.lg-down{
  transition: transform 0.5s ease;
}
.is-focus .lg-down{
  transform: rotate(180deg);
}
.is-focus + .lg-select-dropdown{
  display: block;
}
.lg-select-dropdown{
  width: 100%;
  display: none;
  position: absolute;
  top: 54px;
  left: 0;
  z-index: 1000;
  border: 1px solid #e6e3e3;
  border-radius: 4px;
  background: #fff;
  box-shadow: 0 2px 12px 0 rgba(0,0,0,.1);
  box-sizing: border-box;
  transform-origin: center top 0px;
}
.upper{
  position: absolute;
  top: -12px;
  left: 35px;
  border-width: 6px;
  border-color: transparent;
  border-style: solid;
  border-bottom-color: #e6e3e3;
}
.upper::before{
  content: "";
  position: absolute;
  top: 1px;
  border-width: 6px;
  border-style: solid;
  border-color: transparent;
  border-bottom-color: #fff;
  border-top-width: 0;
  margin-left: -6px;
}
.lg-dropdown{
  position: relative;
  max-height: 286px;
  overflow: hidden;;
  padding: 10px 0;
  box-sizing: border-box;
}
.lg-dropdown-list{
  height: 100%;
  list-style: none;
  overflow: auto;
}
.lg-dropdown-item{
  font-size: 14px;
  padding: 0 20px;
  position: relative;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  color: #606266;
  height: 34px;
  line-height: 34px;
  box-sizing: border-box;
  cursor: pointer;
}
.lg-dropdown-item:hover{
  background: #f3f3f3;
}
</style>
