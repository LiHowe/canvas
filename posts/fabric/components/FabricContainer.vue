<template>
  <div ref="c">
    <canvas ref="canvas" width="500" height="300"></canvas>
    <div class="ops-container" v-if="$slots.ops">
      <slot name="ops"></slot>
    </div>
  </div>
</template>

<script setup lang="ts">
import { fabric } from 'fabric';
import { onMounted, ref } from 'vue'

const canvas = ref<HTMLCanvasElement>()

const fb = ref()

const props = defineProps<{
  mounted?: (fb: typeof fabric, container: fabric.Canvas) => void
  static?: boolean
}>()

const c = ref<HTMLDivElement>()

onMounted(() => {
  fb.value = new fabric[props.static ? 'StaticCanvas' : 'Canvas'](canvas.value!, {
    containerClass: 'fb-container',
    width: c.value?.clientWidth,
  })
  props.mounted && props.mounted(fabric, fb.value)
})
</script>

<style scoped>
.ops-container {
  margin-top: 10px;
  font-size: 12px;
  padding: 10px
}
</style>
