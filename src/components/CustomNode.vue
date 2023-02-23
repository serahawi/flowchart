<script setup>
import { Handle, Position } from '@braks/vue-flow'

const sourceHandleStyleA = { background: '#555', top: '10px' }
const sourceHandleStyleB = { background: '#555', bottom: '10px', top: 'auto' }
</script>

<template>
  <div style="height: 50px; position: relative; display: flex; align-items: center; justify-content: center;" class="vue-flow__node-input">
    Input A

    <Handle
      id="a"
      type="source"
      :position="Position.Right"
      :style="sourceHandleStyleA"
    />
    <Handle
      id="b"
      type="source"
      :position="Position.Right"
      :style="sourceHandleStyleB"
    />
  </div>
</template>