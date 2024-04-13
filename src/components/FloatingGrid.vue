<template>
  <TresMesh :rotation-x='-Math.PI / 2' :position-y='0.01'>
    <TresPlaneGeometry :args='[35, 35]' />
    <TresMeshBasicMaterial
      color='#ffffff'
      :opacity='0.5'
      :transparent='true'
      :map='diffuse'
      :alpha-map='diffuse'
    />
  </TresMesh>
</template>

<script setup lang='ts'>
import * as THREE from 'three'
import { useRenderLoop, useTexture } from '@tresjs/core'

const diffuse = await useTexture([
  '/textures/grid-texture.png'
])
diffuse.wrapS = diffuse.wrapT = THREE.RepeatWrapping
diffuse.anisotropy = 4
diffuse.repeat.set(30, 30)
diffuse.offset.set(0, 0)

const { onLoop } = useRenderLoop()
onLoop(({ elapsed }) => {
  diffuse.offset.set(0, -elapsed * 0.68)
})

</script>
