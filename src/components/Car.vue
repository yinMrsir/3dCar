<template>
  <primitive :object='scene'></primitive>
</template>

<script setup lang='ts'>
import { useGLTF } from '@tresjs/cientos'
import { useRenderLoop } from '@tresjs/core'

const { scene } = await useGLTF('/models/car/scene.gltf')
scene.scale.set(0.005, 0.005, 0.005)
// scene.position.set(0, -0.5, 0)

scene.traverse((child: any) => {
  if (child.isMesh) {
    child.castShadow = true
    child.receiveShadow = true
    child.material.envMapIntensity = 20
  }
})

const { onLoop } = useRenderLoop()
onLoop(({ elapsed }) => {
  const group = scene.children[0].children[0].children[0]
  group.children[0].rotation.x = elapsed * 2
  group.children[2].rotation.x = elapsed * 2
  group.children[4].rotation.x = elapsed * 2
  group.children[6].rotation.x = elapsed * 2
})
</script>
