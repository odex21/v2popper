<template>
  <div class="popper-wrapper">
    <div aria-describedby="tooltip" class="popper-target" ref="target">
      <slot />
    </div>
    <transition name="fade">
      <div
        v-show="visible"
        role="tooltip"
        :class="tooltipClass"
        class="popper tooltip"
        ref="popper"
      >
        <slot name="content" />
        <div v-if="showArrow" class="popper-arrow" data-popper-arrow></div>
      </div>
    </transition>
  </div>
</template>
<script>
import { create } from '../Popover/createPopper'
import './tooltip.styl'
import { popper } from '../utils/popup'
import { on } from '../utils/dom'

export default {
  mixins: [popper],
  props: {
    placement: {
      type: String,
      default: 'top'
    },
    modifiers: {
      type: Array,
      default: () => ([])
    },
    closeOnClickModal: {
      type: Boolean,
      default: false
    },
    appendToBody: {
      type: Boolean,
      default: false
    },
    showEvents: {
      type: Array,
      default: () => (['click', 'focus', 'mouseenter'])
    },
    hideEvents: {
      type: Array,
      default: () => (['mouseleave', 'blur'])
    },
    tabindex: {
      type: Number,
      default: 0
    },
    modal: {
      tyep: Boolean,
      default: false
    },
    effect: {
      type: String,
      default: 'dark',
      validator: e => ['dark'].indexOf(e) > -1
    },
    showArrow: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      instance: null,
      modalAppendToBody: this.appendToBody
    }
  },
  computed: {
    tooltipClass() {
      const res = []
      if (this.effect) res.push(this.effect)
      return res
    }
  },
  mounted() {
    const { placement, modifiers, showEvents, hideEvents, appendToBody } = this
    const { target, popper } = this.$refs
    this.createPopper = create(this.$refs.target, popper, {
      placement,
      modifiers,
    })
    showEvents.forEach(e => on(target, e, () => {
      this.visible = true
    }))
    hideEvents.forEach(e => on(target, e, () => {
      this.close()
    }))
    if (appendToBody) {
      document.body.appendChild(popper)
    }
    target.setAttribute('tabindex', this.tabindex)
  }
}
</script>

<style lang="stylus" scoped>
.popper {
  max-width: 100vw
}
</style>


