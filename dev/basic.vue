<template lang="pug">
.container
  .thing(v-bind:style="style", ref="thing")
  drag-handle(
    @move="move",
    @right="open",
    @aborted="close",
    :disabled="!active || opened",
    :max-right="200",
    style="width: 20px;left:0;background-color:black;opacity:0.5",
    ref="opener"
  )
  drag-handle(
    @move="move",
    @left="close",
    @aborted="open",
    :disabled="!active || !opened",
    :max-left="200",
    :offset="200",
    style="width: 70%;right:0;background-color:black;opacity:0.5",
    ref="closer"
  )
  p &lt;&lt; drag
  div(style="margin-left:200px")
    p the gray area is the drag handle
    p the red area is the drag target
    button(@click="toggle") {{active ? "disable": "enable"}}
    br
    a(href="https://github.com/vue-comps/vue-side-nav/blob/master/dev/basic.vue") source
</template>

<script lang="coffee">
module.exports =
  mixins: [
    require("vue-mixins/vue")
  ]
  created: ->
    @Vue.use(require('vue-touch'))
  data: ->
    active: true
    opened: false
    style:
      top: "0"
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
      @style.left = "0"
      @opened = true
    close: ->
      @style.left = "-200px"
      @opened = false
</script>
