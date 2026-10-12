<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import * as THREE from 'three'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

gsap.registerPlugin(ScrollTrigger)

const host = ref(null)
const progress = ref(0)
let renderer, scene, camera, observer, trigger, raf
let roomGroup, corridorGroup
let reducedMotion = false

function addBox(parent, size, position, material) {
  const mesh = new THREE.Mesh(new THREE.BoxGeometry(...size), material)
  mesh.position.set(...position)
  parent.add(mesh)
  return mesh
}

function addWireBox(parent, size, position, material) {
  const geometry = new THREE.EdgesGeometry(new THREE.BoxGeometry(...size))
  const mesh = new THREE.LineSegments(geometry, material)
  mesh.position.set(...position)
  parent.add(mesh)
  return mesh
}

function resize() {
  if (!host.value || !renderer || !camera) return
  const width = Math.max(1, host.value.clientWidth)
  const height = Math.max(1, host.value.clientHeight)
  renderer.setSize(width, height, false)
  renderer.setPixelRatio(Math.min(window.devicePixelRatio || 1, 1.5))
  camera.aspect = width / height
  camera.fov = width < 700 ? 66 : 54
  camera.updateProjectionMatrix()
}

function setProgress(value) {
  progress.value = value
  if (!camera) return
  const eased = value * value * (3 - 2 * value)
  camera.position.z = 14 - eased * 45
  camera.position.y = 0.05 + Math.sin(value * Math.PI) * 0.12
  camera.rotation.y = Math.sin(value * Math.PI * 2) * 0.018
  camera.rotation.x = Math.sin(value * Math.PI) * -0.012
  if (roomGroup) {
    const reveal = THREE.MathUtils.clamp((value - 0.35) / 0.28, 0, 1)
    roomGroup.traverse((object) => {
      if (object.material && 'opacity' in object.material && object.material.transparent) {
        object.material.opacity = object.userData.baseOpacity * reveal
      }
    })
  }
}

onMounted(() => {
  if (!host.value) return
  reducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  scene = new THREE.Scene()
  scene.background = new THREE.Color('#11120f')
  scene.fog = new THREE.FogExp2('#11120f', 0.018)
  camera = new THREE.PerspectiveCamera(54, 1, 0.1, 100)
  camera.position.set(0, 0, 14)

  renderer = new THREE.WebGLRenderer({ antialias: true, alpha: false, powerPreference: 'low-power' })
  renderer.outputColorSpace = THREE.SRGBColorSpace
  renderer.setPixelRatio(Math.min(window.devicePixelRatio || 1, 1.5))
  host.value.appendChild(renderer.domElement)

  scene.add(new THREE.HemisphereLight(0xc9c7bb, 0x26251f, 1.1))
  const key = new THREE.PointLight(0xd9c5a1, 28, 24)
  key.position.set(0, 1.5, -18)
  scene.add(key)
  const fill = new THREE.PointLight(0x888d88, 14, 22)
  fill.position.set(-4, 2, -9)
  scene.add(fill)

  corridorGroup = new THREE.Group()
  scene.add(corridorGroup)
  const wire = new THREE.LineBasicMaterial({ color: '#888980', transparent: true, opacity: 0.42 })
  const fineWire = new THREE.LineBasicMaterial({ color: '#c7c4b6', transparent: true, opacity: 0.18 })
  for (let i = 0; i < 13; i++) {
    const z = 11 - i * 2.65
    const scale = 1 + i * 0.12
    addWireBox(corridorGroup, [2.3 * scale, 2.3 * scale, 2.3 * scale], [0, 0, z], i % 3 === 0 ? fineWire : wire)
    if (i % 2 === 0) {
      addWireBox(corridorGroup, [1.1 * scale, 1.1 * scale, 1.1 * scale], [0, 0, z - 0.5], fineWire)
    }
  }

  roomGroup = new THREE.Group()
  scene.add(roomGroup)
  const plaster = new THREE.MeshStandardMaterial({ color: '#a7a092', roughness: 0.92, transparent: true, opacity: 1 })
  const darkWood = new THREE.MeshStandardMaterial({ color: '#4a4033', roughness: 0.82, transparent: true, opacity: 0 })
  const linen = new THREE.MeshStandardMaterial({ color: '#c9bda7', roughness: 0.95, transparent: true, opacity: 0 })
  const brass = new THREE.MeshStandardMaterial({ color: '#8f7851', metalness: 0.55, roughness: 0.38, transparent: true, opacity: 0 })
  roomGroup.position.z = -17
  addBox(roomGroup, [8.5, 5.2, 0.22], [0, 0, -7], plaster)
  addBox(roomGroup, [0.22, 5.2, 14], [-4.15, 0, 0], plaster)
  addBox(roomGroup, [0.22, 5.2, 14], [4.15, 0, 0], plaster)
  addBox(roomGroup, [8.5, 0.22, 14], [0, 2.5, 0], darkWood)
  addBox(roomGroup, [8.5, 0.12, 14], [0, -2.5, 0], darkWood)
  addBox(roomGroup, [2.9, 0.12, 1.15], [0, -0.55, -1.5], darkWood)
  addBox(roomGroup, [0.12, 0.95, 0.12], [-1.25, -1.02, -1.5], brass)
  addBox(roomGroup, [0.12, 0.95, 0.12], [1.25, -1.02, -1.5], brass)
  addBox(roomGroup, [0.52, 0.08, 0.52], [-1.65, -1.15, -0.35], darkWood)
  addBox(roomGroup, [0.52, 0.08, 0.52], [1.65, -1.15, -0.35], darkWood)
  addBox(roomGroup, [0.52, 0.08, 0.52], [-1.65, -1.15, -2.65], darkWood)
  addBox(roomGroup, [0.52, 0.08, 0.52], [1.65, -1.15, -2.65], darkWood)
  addBox(roomGroup, [0.72, 0.06, 0.72], [0, -0.45, -1.48], brass)
  addBox(roomGroup, [2.8, 2.8, 0.08], [0, 0.15, -6.84], linen)
  addBox(roomGroup, [2.35, 2.35, 0.04], [0, 0.15, -6.77], plaster)
  addBox(roomGroup, [0.07, 2.5, 0.1], [-2.3, 0.1, -6.7], brass)
  addBox(roomGroup, [0.07, 2.5, 0.1], [2.3, 0.1, -6.7], brass)
  roomGroup.traverse((object) => {
    if (object.material && object.material.transparent) object.userData.baseOpacity = object.material.opacity
  })

  resize()
  observer = new ResizeObserver(resize)
  observer.observe(host.value)

  trigger = ScrollTrigger.create({
    trigger: host.value.closest('.cube-passage'),
    start: 'top top',
    end: 'bottom bottom',
    scrub: reducedMotion ? false : 1.2,
    onUpdate: (self) => setProgress(reducedMotion ? 0.5 : self.progress),
  })
  if (reducedMotion) setProgress(0.5)

  const render = () => {
    raf = requestAnimationFrame(render)
    renderer.render(scene, camera)
  }
  render()
})

onUnmounted(() => {
  cancelAnimationFrame(raf)
  trigger?.kill()
  observer?.disconnect()
  renderer?.dispose()
  if (renderer?.domElement?.parentNode) renderer.domElement.parentNode.removeChild(renderer.domElement)
  scene?.traverse((object) => {
    object.geometry?.dispose?.()
    if (Array.isArray(object.material)) object.material.forEach((material) => material.dispose())
    else object.material?.dispose?.()
  })
})
</script>

<template>
  <section class="cube-passage" aria-label="Immersive architectural passage">
    <div class="cube-passage-sticky">
      <div ref="host" class="cube-passage-canvas" aria-hidden="true"></div>
      <div class="passage-top"><span>SPATIAL STUDY / 002</span><span>FORM · SEQUENCE · ATMOSPHERE</span></div>
      <div class="passage-copy">
        <p class="passage-kicker">Architecture shapes perception</p>
        <h2 v-if="progress < 0.42">A passage<br /><em>through space.</em></h2>
        <h2 v-else-if="progress < 0.78">Arrive.<br /><em>Stay a while.</em></h2>
        <h2 v-else>Space is<br /><em>an experience.</em></h2>
        <p class="passage-hint">{{ progress < 0.4 ? 'A sequence of structure and light.' : progress < 0.78 ? 'The tea room · a moment of stillness.' : 'Continue to explore the studio.' }}</p>
      </div>
      <div class="passage-bottom"><span>REAL-TIME 3D / WEBGL</span><span>SCROLL TO MOVE <b>{{ String(Math.round(progress * 100)).padStart(2, '0') }}</b></span></div>
      <div class="passage-progress"><span :style="{ transform: `scaleX(${progress})` }"></span></div>
    </div>
  </section>
</template>
