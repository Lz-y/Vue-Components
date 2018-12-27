<template>
  <div class="lg-input__container">
    <span class="lg-input__prefix">
      <i class="iconfont" :class="icon"></i>
    </span>
    <template v-if="type !== 'textarea'">
      <input :type="type" class="lg-input__inner" :class="[icon === undefined ? 'pl': '']"
      :placeholder="placeholder" :value="value" :readonly='readonly' @input="enter" @keyup.enter="search"
      :required='rule.required' @blur="blur">
    </template>
    <template v-else>
      <textarea rows="3" :value="value" :readonly='readonly' :placeholder="placeholder" @input="enter"
      class="lg-textarea__inner"></textarea>
    </template>
    <span class="lg-input__suffix" v-if="show" @click="clear">
      <i class="iconfont" :class="suffix"></i>
    </span>
    <span class="lg-input__tip" v-if="showTip">{{rule.message}}</span>
  </div>
</template>

<script>
export default {
  name: 'lg-input',
  props: {
    'value': [Number, String],
    'readonly': {
      type: Boolean,
      default: false
    },
    'placeholder': [String],
    'type': {
      type: String,
      default: 'text'
    },
    'icon': [String],
    'rule': {
      type: Object,
      default: function () {
        return {
          'required': false,
          'message': ''
        }
      }
    },
    'suffix': {
      type: String,
      default: 'lg-close'
    }
  },
  data () {
    return {
      show: false,
      showTip: false
    }
  },
  methods: {
    enter (event) {
      let val = event.target.value
      if (val && val.length > 0) {
        this.show = true
        this.showTip = false
      }
      this.$emit('input', val)
    },
    blur (event) {
      this.showTip = event.target.validity.valid === false
    },
    search () {
      if (this.show) {
        this.$emit('search')
      }
    },
    clear () {
      this.show = false
      this.showTip = false
      this.$emit('input', undefined)
    }
  }
}
</script>

<style scoped>
.lg-input__container{
  position: relative;
  width: 300px;
  box-sizing: border-box;
}
.lg-input__prefix, .lg-input__suffix{
  position: absolute;
  color: #9e9191;
}
i[class*='lg-']{
  height: 36px;
  width: 30px;
  display: inline-block;
  line-height: 36px;
  text-align: center;
}
.lg-input__inner{
  border: 1px solid #ddd;
  height: 36px;
  line-height: 36px;
  width: 100%;
  padding: 0 30px;
  display: inline-block;
  box-sizing: border-box;
  border-radius: 4px;
}
.lg-input__inner.pl{
  padding-left: 10px;
}
.lg-textarea__inner{
  width: 100%;
  line-height: 1.5;
  border: 1px solid #ddd;
  outline: none;
  box-sizing: border-box;
  border-radius: 4px;
  padding: 8px 10px;
  resize: vertical;
}
input:focus, .lg-textarea__inner:focus{
  border: 1px solid #07b6e0;
  outline: none;
}
.lg-input__suffix{
  right: 0;
  top: 0;
}
.lg-input__tip{
  display: inline-block;
  font-size: 12px;
  line-height: 1;
  color: #f70303;
  padding-top: 4px;
  position: absolute;
  top: 100%;
  left: 0;
}
</style>
