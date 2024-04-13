<template>
  <TresMesh
    ref='itemRef'
    receive-shadow
    cast-shadow
    v-for='item in 14'>
    <TresTorusGeometry :args='[3.35, 0.05, 16, 100]' />
    <TresMeshStandardMaterial
      color='rgb(0, 0, 0)'
      emissive='pink'
      emissive-intensity='5'
    />
  </TresMesh>
</template>

<script setup lang='ts'>
import * as THREE from 'three'
import { ref } from 'vue'
import { useRenderLoop } from '@tresjs/core'

const itemRef = ref<InstanceType<typeof THREE.Mesh>[]>([])

const { onLoop } = useRenderLoop()
onLoop(({ elapsed }) => {
  itemRef.value.forEach((item, index) => {
    // [-7, 6]
    let z = (index - 7) * 3.5 - (elapsed * 0.8) % 7
    item.position.set(0, 0, z)

    let dist = Math.abs(z)
    item.scale.set(1 - dist * 0.04, 1 - dist * 0.04, 1 - dist * 0.04)

    let colorScale = 1
    if (dist > 2) {
      colorScale = 1 - (Math.min(dist, 12) - 2) /10
    }
    colorScale *= 0.5

    if (index % 2 === 0) {
      item.material.emissive = new THREE.Color(6, 0.15, 0.7).multiplyScalar(colorScale)
    } else {
      item.material.emissive = new THREE.Color(0.1, 0.7, 3).multiplyScalar(colorScale)
    }
  })
})
</script>
