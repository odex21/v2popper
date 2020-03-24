<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,
      <br />check out the
      <a
        href="https://cli.vuejs.org"
        target="_blank"
        rel="noopener"
      >vue-cli documentation</a>.
    </p>
    <button @click="drawer = true">showDrawer</button>
    <h-tooltip :hideEvents="[]">
      tooltip
      <div slot="content">test</div>
    </h-tooltip>
    <div class="test-wrapper">
      <h-drawer :showDrawer.sync="drawer">
        something
        <h-tooltip>
          tooltip
          <div slot="content">test</div>
        </h-tooltip>
      </h-drawer>
      <h-popover>
        Test1
        <div slot="content">
          <div class="test-popover">
            <h-popover>
              Test2
              <div slot="content">
                <div class="test-popover">
                  <h-popover>
                    Test3
                    <div slot="content">Test4</div>
                  </h-popover>
                </div>
              </div>
            </h-popover>
          </div>
        </div>
      </h-popover>
    </div>
  </div>
</template>

<script>
import HPopover from './Popover'
import HTooltip from './Tooltip'
import HDrawer from './Drawer'

export default {
  name: 'HelloWorld',
  components: {
    HPopover,
    HTooltip,
    HDrawer
  },
  props: {
    msg: String
  },
  data() {
    return {
      drawer: false,
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus">
h3 {
  margin: 40px 0 0
}

ul {
  list-style-type: none
  padding: 0
}

li {
  display: inline-block
  margin: 0 10px
}

a {
  color: #42b983
}

.hello {
  height: 200vh
}

.test-wrapper {
  margin-top: 200px
}

.test-popover {
  width: 200px
  height: 400px
}
</style>
