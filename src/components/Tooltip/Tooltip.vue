<template>
  <div class="popper-wrapper">
    <div aria-describedby="tooltip" class="popper-target" ref="target">
      <slot />
    </div>
    <transition name="fade">
      <div v-show="visible" role="tooltip" class="popper el-tooltip" ref="popper">
        <slot name="content" />
        <div class="popper-arrow" data-popper-arrow></div>
      </div>
    </transition>
  </div>
</template>
<script>
import { create } from '../Popover/createTooltip'
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
    }
  },
  data() {
    return {
      instance: null,
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
      setTimeout(() => {
        this.popperInstance = createPopper()
      }, 100)
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


