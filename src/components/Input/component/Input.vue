<template>
  <input
    v-if="type!=='textarea'"
    ref="input"
    :class="`form-control form-focus-${color}`"
    :value="nowValue"
    :type="type"
    v-bind="$props"
    :placeholder="placeholder"
    :disabled="disabled"
    @input="inputEvent"
    @focus="focusEvent"
    @blur="blurEvent"
  >
</template>
<script>
  /**
   *
   * */
  export default {
    name: 'FInput',
    data () {
      return {
        nowValue: ''
      }
    },
    props: {
      placeholder: String,
      type: {
        type: String,
        default: 'text'
      },
      color: {
        type: String,
        default: 'success'
      },
      value: [String, Number],
      disabled: {
        type: Boolean,
        default: false
      }
    },
    mounted () {
      this.setNewValue(this.value)
    },
    watch: {
      'value' (val, oldValue) {
        this.setNewValue(val)
      }
    },
    methods: {
      setNewValue (value) {
        if (value === this.nowValue) return
        this.nowValue = value
      },
      inputEvent (evt) {
        const value = evt.target.value
        this.$emit('input', value)
        this.setNewValue(value)
        this.$emit('change', value)
      },
      focusEvent (evt) {
        this.$emit('focus', evt)
        if (this.$parent.focusEvent) this.$parent.focusEvent(this.color)
      },
      blurEvent (evt) {
        this.$emit('blur', evt)
        if (this.$parent.blurEvent) this.$parent.blurEvent()
      }
    }
  }
</script>
