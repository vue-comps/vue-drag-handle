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

  el: -> document.createElement "div"

  props:
    "factor":
      type:Number
      default: 2
    "left":
      type: Boolean
      default: true
    "maxWidth":
      type:Number
      required: true
    "widthOpened":
      type:String
      default:"70%"
    "widthClosed":
      type:String
      default:"20px"
    "dismissable":
      type:Boolean
      default: true
    "zIndex":
      type:Number
      default: 1002
    "isOpened":
      type:Boolean
      default: false
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
    opened: false

  compiled: ->
    @send()

  watch:
    "zIndex": (val) -> @style.zIndex = val
    "isOpened": (val) ->
      if val != @opened
        if val
          @open(false)
        else
          @close(false)

  methods:
    dismiss: (e) ->
      if !@disabled and @isOpened and @dismissable and not e.defaultPrevented
        @close()
        e.preventDefault()
    send: ->
      if (@isOpened and !@left) or (!@isOpened and @left)
        @style.right = undefined
        @style.left = 0
      else
        @style.right = 0
        @style.left = undefined
    open: (emit=true) ->
      @opened = true
      @isOpened = true
      @style.width = @widthOpened
      @send()
      @$emit "opened" if emit
    close: (emit=true) ->
      @opened = false
      @isOpened = false
      @style.width = @widthClosed
      @send()
      @$emit "closed" if emit
    onPan: (e) ->
      if !@disabled and e.type == "pan"
        e.preventDefault()
        e.srcEvent.stopPropagation()
        dX = e.deltaX*@factor
        dX = -dX unless @left
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
</script>
