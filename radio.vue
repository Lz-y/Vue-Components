<template>
  <label role="radio" aria-checked="true" class="lg-radio"
  :class="{'checked': checked, 'disabled': disabled}">
    <span class="lg-radio__icon"></span>
    <input type="radio" ref="radio" class="lg-radio__original" :value="label" @click="select"
    :disabled='disabled'>
    <span class="lg-radio__label">
      <slot></slot>
    </span>
  </label>
</template>

<script>
export default {
  name: 'lg-radio',
  model: {
    prop: 'model',
    event: 'change'
  },
  props: {
    label: {
      type: [Number, String],
      required: true
    },
    model: {
      type: [Number, String],
      required: true
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      checked: false
    }
  },
  methods: {
    select () {
      this.$emit('change', this.$refs.radio.value)
    }
  },
  watch: {
    model: function (val) {
      this.checked = val === this.label
    }
  },
  mounted () {
    this.select()
  }
}
</script>

<style scoped>
.lg-radio{
  font-size: 14px;
  color: #4a4949;
  font-weight: 500;
  cursor: pointer;
  outline: none;
  display: inline-block;
  text-align: left;
  margin: 0 20px 0 0;
}
.lg-radio.checked{
  color: #21b4e0;
}
.lg-radio.disabled {
  border-color: #ccc;
  opacity: 0.6;
  cursor: no-drop;
}
.lg-radio__icon {
  width: 14px;
  height: 14px;
  display: inline-block;
  border: 1px solid #c5c5c5;
  border-radius: 50%;
  margin-right: 6px;
  vertical-align: -2px;
  position: relative;
}
.lg-radio__icon::after{
  content: '';
  position: absolute;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  display: inline-block;
  top: 3px;
  left: 3px;
  background: rgba(33, 180, 224, 0);
  transform: scale(0);
  transition: all 0.5s ease;
}
.lg-radio.checked .lg-radio__icon{
  border-color: #21b4e0;
}
.lg-radio.checked .lg-radio__icon::after{
  background: rgba(33, 180, 224, 1);
  transform: scale(1);
}
.lg-radio__original {
  display: none;
  outline: medium none!important;
}
</style>
