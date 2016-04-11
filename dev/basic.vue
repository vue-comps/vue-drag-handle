<template lang="jade">
.container
  .thing(v-bind:style="style")
  drag-handle(
    @move="move"
    @opened="open"
    @open-aborted="close"
    @closed="close"
    @close-aborted="open"
    v-bind:disabled="!active"
    v-bind:max-width="200"
  )
  p &lt;&lt; drag
  p(style="left:200px;top:30px;position:relative") the gray area is the drag handle
  p(style="left:200px;top:30px;position:relative") the red area is the drag target
  button(@click="toggle" style="left:200px;top:30px;position:relative") toggle
  a(href="https://github.com/vue-comps/vue-side-nav/blob/master/dev/basic.vue") source
</template>

<script lang="coffee">
module.exports =
  mixins: [
    require("vue-mixins/getVue")
  ]
  beforeCompile: ->
    Vue = @getVue()
    Vue.use(require('vue-touch'))
  data: ->
    active: true
    style:
      top: 0
      width: "200px"
      left: "-200px"
      height: "100%"
      position:"absolute"
      "background-color":"red"
  components:
    "drag-handle" : require "../src/drag-handle.vue"
  methods:
    toggle: ->
      @active = !@active
    move: (position) ->
      @style.left = -200+position+ "px"
    open: ->
      @style.left = 0
    close: ->
      @style.left = "-200px"
</script>

<style lang="stylus">
.container > a
  position absolute
  left 250px
  top 40px
.vc-drag-handle
  background-color: black
  opacity 0.5
</style>
