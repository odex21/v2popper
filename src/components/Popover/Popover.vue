<template>
  <div class="popper-wrapper">
    <div targe aria-describedby="popover" class="popper-target" ref="target">
      <slot />
    </div>
    <transition name="fade">
      <div v-show="visible" role="tooltip" class="popper el-popover" ref="popper">
        <slot name="content" />
        <div class="popper-arrow" data-popper-arrow></div>
      </div>
    </transition>
  </div>
</template>
<script>
import { create } from './createTooltip'
import './popover.styl'
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
    showEvents: {
      type: Array,
      default: () => (['click', 'focus'])
    },
    hideEvents: {
      type: Array,
      default: () => ([])
    },
    tabindex: {
      type: Number,
      default: 0
    },
    closeOnClickModal: {
      type: Boolean,
      default: true
    },
    appendToBody: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      popperInstance: null,
      modalAppendToBody: this.appendToBody
    }
  },
  mounted() {
    const { placement, modifiers, showEvents, hideEvents, appendToBody } = this
    const { target, popper } = this.$refs
    const createPopper = create(this.$refs.target, popper, {
      placement,
      modifiers,
      showEvents,
      hideEvents
    })
    showEvents.forEach(e => on(target, e, () => {
      this.visible = true
      this.popperInstance = createPopper()
    }))
    hideEvents.forEach(e => on(target, e, () => {
      this.visible = false

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


