<template lang="jade">
.container
  .thing(v-bind:style="style")
  drag-handle(
    @move="move"
    @right="open"
    @aborted="close"
    v-bind:disabled="!active || opened"
    v-bind:max-right="200"
    style="width: 20px;left:0;"
  )
  drag-handle(
    @move="move"
    @left="close"
    @aborted="open"
    v-bind:disabled="!active || !opened"
    v-bind:max-left="200"
    v-bind:offset="200"
    style="width: 70%;right:0;"
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
    opened: false
    style:
      top: 0
      width: "200px"
      left: "-200px"
      height: "100%"
      position:"absolute"
      backgroundColor:"red"
  components:
    "drag-handle" : require "../src/drag-handle.vue"
  methods:
    toggle: ->
      @active = !@active
    move: (position) ->
      @style.left = -200+position+ "px"
    open: ->
      @style.left = 0
      @opened = true
    close: ->
      @style.left = "-200px"
      @opened = false
</script>

<style lang="stylus">
.container > a
  position absolute
  left 250px
  top 40px
.drag-handle
  background-color: black
  opacity 0.5
</style>
