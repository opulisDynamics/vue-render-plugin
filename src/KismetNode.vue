<template lang="pug">
.node(:class="[selected(), node.name, node.tag?'node-'+node.tag:'node'] | kebab")
  .title 
    span {{ node.DisplayName || node.name}}

  // Inputs (before outputs to auto-flow them to top)
  .input(v-for='input in inputs()')
    Socket(v-socket:input="input", type="input", :socket="input.socket" :connected="input.hasConnection()")
    .input-title {{input.name}}
    .input-control(
      v-show='input.showControl()'
      v-control="input.control"
    )

  // Outputs
  .output(v-for='output in outputs()')
    .output-title {{output.name}}
    Socket(v-socket:output="output", type="output", :socket="output.socket" :connected="output.hasConnection()")

  // Controls
  .control(
    v-for='control in controls()',
    v-control="control"
  )

  
</template>

<script>
import mixin from './mixin';
import Socket from './Socket.vue';

export default {
  mixins: [mixin],
  components: {
    Socket
  }
}
</script>

<style lang="sass" scoped>
@import "./vars"

.node
  background: $node-color
  border: 2px solid #4e58bf
  border-radius: 10px
  cursor: pointer
  min-width: $node-width
  height: auto
  padding-bottom: 6px
  box-sizing: content-box
  position: relative
  user-select: none
  display: grid
  grid-template-columns: repeat(2, auto)
  grid-template-rows: 34px repeat(auto-fill, minmax(34px, 1fr))
  grid-auto-flow: dense
  &:hover
    background: lighten($node-color,4%)
  &.selected
    background: $node-color-selected
    border-color: #e3c000
  .title
    color: white
    font-size: 18px
    padding: 8px
    grid-column: 1 / 3
    grid-row: 1 / 2
  .output
    text-align: right
    grid-column: 2 / 3
  .input
    text-align: left
    grid-column: 1 / 2
  .input-title,.output-title
    vertical-align: middle
    color: white
    display: inline-block
    font-family: sans-serif
    font-size: 14px
    margin: $socket-margin
    line-height: $socket-size
  .input-control
    z-index: 1
    vertical-align: middle
    display: inline-block
  .control
    padding: $socket-margin $socket-size/2 + $socket-margin
    grid-column: 1 / 3
</style>