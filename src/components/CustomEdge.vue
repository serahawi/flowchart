<script setup>
import { useTransition, TransitionPresets } from '@vueuse/core'
import { getSmoothStepPath, useVueFlow } from '@braks/vue-flow'
import { computed, watch } from 'vue'

const props = defineProps({
  id: {
    type: String,
    required: true,
  },
  source: {
    type: String,
    required: true
  },
  target: {
    type: String,
    required: true,
  },
  sourceX: {
    type: Number,
    required: true,
  },
  sourceY: {
    type: Number,
    required: true,
  },
  targetX: {
    type: Number,
    required: true,
  },
  targetY: {
    type: Number,
    required: true,
  },
  sourcePosition: {
    type: String,
    required: true,
  },
  targetPosition: {
    type: String,
    required: true,
  },
  data: {
    type: Object,
    required: false,
  },
  markerEnd: {
    type: String,
    required: false,
  },
  style: {
    type: Object,
    required: false,
  },
})

const curve = ref()

const dot = ref()

const transform = ref({})

const showDot = ref(false)

const { onNodeDoubleClick } = useVueFlow()

const edgePath = computed(() =>
  getSmoothStepPath({
    sourceX: props.sourceX,
    sourceY: props.sourceY,
    sourcePosition: props.sourcePosition,
    targetX: props.targetX,
    targetY: props.targetY,
    targetPosition: props.targetPosition,
  }), 
)

onNodeDoubleClick(({ node }) => {
  const isSource = props.source === node.id
  const isTarget = props.target === node.id

  if (!showDot.value && (isSource || isTarget)) {
    showDot.value = true
    let totalLength = curve.value.getTotalLength()
    const initialPos = ref(isSource ? 0 : totalLength)

    const output = useTransition(initialPos, {
      duration: 4000,
      transition: TransitionPresets.easeOutCubic,
      onFinished: () => {
        stop()
        showDot.value = false
      }
    })

    initialPos.value = isSource ? totalLength : 0

    const stop = watch(output, (next) => {
      const nextLength = curve.value.getTotalLength()

      if (totalLength !== nextLength) {
        totalLength = nextLength
        initialPos.value = isSource ? totalLength : 0
      }

      transform.value = curve.value.getPointAtLength(next)
    })
  }
})
</script>

<script>
export default {
  inheritAttrs: false,
}
</script>

<template>
  <path ref="curve" :id="id" :style="style" class="vue-flow__edge-path" :d="edgePath" :marker-end="markerEnd" />
  <Transition name="fade">
    <circle
      v-if="showDot"
      ref="dot"
      r="5"
      cy="0"
      cx="0"
      :transform="`translate(${transform.x}, ${transform.y})`"
      style="fill: #FDD023" 
    />
  </Transition>
</template>
