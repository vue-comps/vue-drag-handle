// out: ..
<template lang="jade">
.vc-drag-handle(
  v-bind:style="style"
  @click="dismiss"
  @keyup.esc="dismiss"
  v-touch:pan="onPan"
  v-bind:class="{disabled:disabled}"
  )
</template>

<script lang="coffee">
module.exports =

  mixins:[
    require("vue-mixins/isOpened")
  ]

  props:
    "factor":
      type:Number
      default: 2
    "right":
      type: Boolean
      default: false
    "maxWidth":
      type:Number
      required: true
    "widthOpened":
      type:String
      default:"70%"
    "widthClosed":
      type:String
      default:"20px"
    "notDismissible":
      type:Boolean
      default: false
    "zIndex":
      type:Number
      default: 1002
    "disabled":
      type: Boolean
      default: false
  data: ->
    style:
      height: "100%"
      position: "absolute"
      top: 0
      zIndex: @zIndex
      right: undefined
      left: undefined
      width: @widthClosed

  compiled: ->
    @send()

  watch:
    "zIndex": (val) -> @style.zIndex = val

  methods:
    dismiss: (e) ->
      if !@disabled and @isOpened and not @notDismissible and not e.defaultPrevented
        @close()
        e.preventDefault()
    send: ->
      if (@opened and @right) or (!@opened and !@right)
        @style.right = undefined
        @style.left = 0
      else
        @style.right = 0
        @style.left = undefined
    open: (emit=true) ->
      @setOpened()
      @style.width = @widthOpened
      @send()
      @$emit "opened" if emit
    close: (emit=true) ->
      @setClosed()
      @style.width = @widthClosed
      @send()
      @$emit "closed" if emit
    onPan: (e) ->
      if !@disabled and e.type == "pan"
        e.preventDefault()
        e.srcEvent.stopPropagation()
        dX = e.deltaX*@factor
        dX = -dX if @right
        if e.isFinal
          if @opened
            if dX <= -@maxWidth
              @close()
            else
              @$emit "close-aborted"
          else
            if dX >= @maxWidth
              @open()
            else
              @$emit "open-aborted"
        else
          if @opened
            if dX< 0 and dX >= -@maxWidth
              @$emit "move", @maxWidth+dX
          else
            if dX > 0 and dX <= @maxWidth
              @$emit "move", dX
    toggle: ->
      if @opened
        @close(false)
      else
        @open(false)
</script>
