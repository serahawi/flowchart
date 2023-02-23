<script lang="ts" setup>
import {
  ConnectionMode,
  Controls,
  Elements,
  isEdge,
  MiniMap,
  Node,
  Position,
  SnapGrid,
  useVueFlow,
  VueFlow,
} from '@braks/vue-flow'
import CustomEdge from './CustomEdge.vue'
import CustomNode from './CustomNode.vue'

const elements = ref<Elements>([
  {
      id: '1',
      type: 'custom',
      position: { x: 0, y: 50 },
      style: {
        height: '150px',
      },
      sourcePosition: Position.Right,
    },
    {
      id: '2',
      type: 'output',
      label: 'Output A',
      position: { x: 500, y: 0 },
      targetPosition: Position.Left,
    },
    {
      id: '3',
      type: 'output',
      label: 'Output B',
      position: { x: 500, y: 125 },
      targetPosition: Position.Left,
    },

    { id: 'e1-2', type: 'custom', source: '1', sourceHandle: 'a', target: '2', style: { stroke: '#fff' } },
    { id: 'e1-3', type: 'custom', source: '1', sourceHandle: 'b', target: '3', style: { stroke: '#fff' } },
])
const bgColor = ref('#1A192B')
const connectionLineStyle = { stroke: '#fff' }
const snapGrid: SnapGrid = [16, 16]

const { onPaneReady } = useVueFlow({
  connectionMode: ConnectionMode.Loose,
  connectionLineStyle,
  snapToGrid: true,
  snapGrid,
  defaultZoom: 1.5,
})

onPaneReady((i) => {
  i.fitView()
  console.log('flow loaded:', i)
})
</script>
<template>
  <VueFlow v-model="elements" :style="{ backgroundColor: bgColor }">
    <template #node-custom="props">
      <CustomNode v-bind="props" />
    </template>
    
    <template #edge-custom="props">
      <CustomEdge v-bind="props" />
    </template>
  </VueFlow>
</template>
