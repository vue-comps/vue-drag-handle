// out: ..
<template lang="jade">
.vc-drag-handle(
  v-bind:style="style"
  @click="dismiss | notPrevented | prevent"
  @keyup.esc="dismiss | notPrevented | prevent"
  v-touch:pan="onPan"
  )
</template>

<script lang="coffee">
module.exports =

  filters:
    notPrevented: require("vue-filters/notPrevented")
    prevent: require("vue-filters/prevent")

  el: -> document.createElement "div"

  props:
    "factor":
      type:Number
      default: 2
    "left":
      type: Boolean
      default: true
    "onMove":
      type: Function
      required: true
    "onOpen":
      type: Function
      required: true
    "onOpenAbort":
      type: Function
    "onClose":
      type: Function
      required: true
    "onCloseAbort":
      type: Function
    "maxOpen":
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
      default: 1001

  data: ->
    style:
      height: "100%"
      position: "absolute"
      top: 0
      "z-index": @zIndex
      right: undefined
      left: undefined
      width: @widthClosed
    opened: false

  compiled: ->
    @onCloseAbort = @onOpen unless @onCloseAbort?
    @onOpenAbort = @onClose unless @onOpenAbort?
    @send()

  watch:
    "zIndex": (val) -> @style["z-index"] = val

  methods:
    dismiss: -> @close() if @dismissable
    send: ->
      if (@opened and !@left) or (!@opened and @left)
        @style.right = undefined
        @style.left = 0
      else
        @style.right = 0
        @style.left = undefined
    open: ->
      @opened = true
      @style.width = @widthOpened
      @send()
      @onOpen()
    close: ->
      @opened = false
      @style.width = @widthClosed
      @send()
      @onClose()
    onPan: (e) ->
      if e.type == "pan"
        e.preventDefault()
        e.srcEvent.stopPropagation()
        dX = e.deltaX*@factor
        dX = -dX unless @left
        if e.isFinal
          if @opened
            if dX <= -@maxOpen
              @close()
            else
              @onCloseAbort()
          else
            if dX >= @maxOpen
              @open()
            else
              @onOpenAbort()
        else
          if @opened
            if dX< 0 and dX >= -@maxOpen
              @onMove(@maxOpen+dX)
          else
            if dX > 0 and dX <= @maxOpen
              @onMove(dX)
</script>
