<script lang="ts" setup>
import { Graphics } from 'pixi.js'
import { computed, ref } from 'vue'
import { useScreen } from 'vue3-pixi'

const screen = useScreen()

const scale = computed(() => {
  const { width, height } = screen.value
  return Math.min(width / 320, height / 480)
})
const position = computed(() => {
  const { width, height } = screen.value
  return {
    x: (width - 320 * scale.value) / 2,
    y: (height - 480 * scale.value) / 2,
  }
})

const mask = ref<Graphics>()

function drawMask(mask: Graphics) {
  mask.beginFill(0xFFFFFF).drawRect(0, 0, 320, 480).endFill()
}
</script>

<template>
  <container :x="position.x" :y="position.y" :scale="scale">
    <graphics ref="mask" is-mask @draw="drawMask" />
    <container :mask="mask">
      <slot />
    </container>
  </container>
</template>

<style lang="scss" scoped></style>
