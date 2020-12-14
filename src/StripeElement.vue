<template>
    <div ref="root" />
</template>

<script>
import props from './props'
import { create, destroy } from './stripeElements'

export default {
  // please see https://stripe.com/docs/elements/reference for details
  props: Object.assign({type: {type:String, required:true}}, props),

    emits: ['blur', 'focus', 'change'],

  beforeMount () {
    this._element = create(this.type, this.stripe, this.options)
    this._element.on('blur', event => this.$emit('blur'))
    this._element.on('focus', event => this.$emit('focus'))
    this._element.on('change', event => this.$emit('change', event))
  },

  mounted () {
      const el = document.createElement('div')

      this._element.mount(el)

      this.$refs['root'].appendChild(el)
  },

    beforeUnmount () {
    this._element.unmount()
    this._element.destroy()
    destroy()
  },

  methods: {
    blur () { this._element.blur() },
    clear () { this._element.clear() },
    focus () { this._element.focus() },
    update () { this._element.update() }
  }
}
</script>
