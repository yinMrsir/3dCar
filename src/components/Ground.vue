<template>
  <Plane receive-shadow :args='[30, 30]'>
    <TresMeshStandardMaterial
      :env-map-intensity="0"
      :normal-map="textures[1]"
      :normal-scale="new THREE.Vector2(0.15, 0.15)"
      :roughness-map='textures[0]'
      :dithering='true'
      :color='[0.015, 0.015, 0.015]'
      :roughness='0.7'
      :metalness='1'
    />
  </Plane>
</template>

<script setup lang='ts'>
import * as THREE from 'three'
import { Plane } from '@tresjs/cientos'
import { useRenderLoop, useTexture } from '@tresjs/core'

const textures = await useTexture([
  '/textures/terrain-roughness.jpg',
  '/textures/terrain-normal.jpg'
])

textures.forEach((t) => {
  t.wrapS = t.wrapT = THREE.RepeatWrapping
  t.repeat.set(5, 5)
  t.offset.set(0, 0)
})

const { onLoop } = useRenderLoop()
onLoop(({ elapsed }) => {
  const time = -elapsed * 0.128
  textures.forEach((t) => {
    t.offset.set(0, time % 1)
  })
})

</script>
