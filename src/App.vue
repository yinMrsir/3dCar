<script setup lang="ts">
import * as THREE from 'three'
import { TresCanvas, useRenderLoop } from '@tresjs/core'
import { OrbitControls } from '@tresjs/cientos'
import Car from '@/components/Car.vue'
import Ground from '@/components/Ground.vue'
import FloatingGrid from '@/components/FloatingGrid.vue'
import Rings from '@/components/Rings.vue'
import { ref, shallowRef, watch } from 'vue'

const fbo = shallowRef(new THREE.WebGLCubeRenderTarget(256))
fbo.value.texture.type = THREE.HalfFloatType

const camera = new THREE.CubeCamera(1, 1000, fbo.value)
camera.position.set(0, 0, 0)

const canvasRef = shallowRef<InstanceType<typeof TresCanvas>>()
const { onLoop } = useRenderLoop()

watch(fbo.value, () => {
  fbo.value.dispose()
})

const carVisible = ref(true)
let count = 0
onLoop(() => {
  if (canvasRef.value?.context && count > 5 ) {
    const { scene, renderer } = canvasRef.value.context
    carVisible.value = false
    camera.update(renderer.value, scene.value)
    scene.value.environment = fbo.value.texture
    carVisible.value = true

    count = 0
  }

  count++
})
</script>

<template>
  <TresCanvas ref='canvasRef' shadows window-size clear-color='rgb(0, 0, 0)'>
    <TresPerspectiveCamera :position='[3, 2, 5]' :fov='50' />
    <OrbitControls :target="[0 ,0.35, 0]" :max-polar-angle="1.45" />
    <Suspense>
      <Car v-if='carVisible' />
    </Suspense>
    <Suspense>
      <Ground/>
    </Suspense>
    <Suspense>
      <FloatingGrid/>
    </Suspense>
    <Rings/>
    <TresSpotLight
      :color='[1, 0.25, 0.7]'
      :intensity='150'
      :angle='0.6'
      :penumbra='0.5'
      :position='[5, 5, 0]'
      cast-shadow
    />
    <TresSpotLight
      :color='[0.14, 0.5, 1]'
      :intensity='200'
      :angle='0.6'
      :penumbra='0.5'
      :position='[-5, 5, 0]'
      cast-shadow
    />
    <TresAxesHelper />
  </TresCanvas>
</template>

<style>
html,
body {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
}
#app {
  height: 100%;
  width: 100%;
}
</style>
