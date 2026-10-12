<script setup>
import { onMounted, onUnmounted, ref } from 'vue'
import * as THREE from 'three'

const host = ref(null)
const exploded = ref(false)
const ready = ref(false)
let renderer, scene, camera, animationFrameId, observer, root, doorAssembly
let pointerDown = false
let lastX = 0
let lastY = 0
let targetRotationY = -0.28
let targetRotationX = -0.08
let currentRotationY = targetRotationY
let currentRotationX = targetRotationX
const parts = []

function makeLine(points, material, parent = root) {
  const geometry = new THREE.BufferGeometry().setFromPoints(points.map(([x, y, z]) => new THREE.Vector3(x, y, z)))
  const line = new THREE.Line(geometry, material)
  parent.add(line)
  return line
}

function makeBox(w, h, d, material, x, y, z, parent = doorAssembly) {
  const mesh = new THREE.Mesh(new THREE.BoxGeometry(w, h, d), material)
  mesh.position.set(x, y, z)
  parent.add(mesh)
  return mesh
}

function setExploded(value) {
  exploded.value = value
  parts.forEach((part) => {
    const distance = value ? part.offset : 0
    part.mesh.position.set(part.base.x, part.base.y, part.base.z + distance)
  })
}

function resize() {
  if (!host.value || !renderer || !camera) return
  const width = Math.max(1, host.value.clientWidth)
  const height = Math.max(1, host.value.clientHeight)
  renderer.setSize(width, height, false)
  renderer.setPixelRatio(Math.min(window.devicePixelRatio || 1, 1.7))
  camera.aspect = width / height
  camera.position.z = width < 600 ? 8.5 : 7.1
  camera.updateProjectionMatrix()
}

function onPointerDown(event) {
  pointerDown = true
  lastX = event.clientX
  lastY = event.clientY
  host.value?.setPointerCapture?.(event.pointerId)
}
function onPointerMove(event) {
  if (!pointerDown) return
  const dx = event.clientX - lastX
  const dy = event.clientY - lastY
  targetRotationY += dx * 0.008
  targetRotationX = THREE.MathUtils.clamp(targetRotationX + dy * 0.005, -0.65, 0.65)
  lastX = event.clientX
  lastY = event.clientY
}
function onPointerUp() { pointerDown = false }

onMounted(() => {
  if (!host.value) return
  scene = new THREE.Scene()
  scene.background = new THREE.Color('#d9d8d2')
  camera = new THREE.PerspectiveCamera(32, 1, 0.1, 100)
  camera.position.set(0, 0, 7.1)
  renderer = new THREE.WebGLRenderer({ antialias: true, alpha: false, powerPreference: 'low-power' })
  renderer.outputColorSpace = THREE.SRGBColorSpace
  renderer.setPixelRatio(Math.min(window.devicePixelRatio || 1, 1.7))
  host.value.appendChild(renderer.domElement)

  scene.add(new THREE.HemisphereLight(0xfaf9f1, 0x7c7d76, 2.1))
  const key = new THREE.DirectionalLight(0xffffff, 2.2)
  key.position.set(-3, 5, 6)
  scene.add(key)
  const rim = new THREE.DirectionalLight(0xaaa99f, 1.4)
  rim.position.set(4, 1, -4)
  scene.add(rim)

  root = new THREE.Group()
  scene.add(root)
  doorAssembly = new THREE.Group()
  root.add(doorAssembly)

  const darkMetal = new THREE.MeshStandardMaterial({ color: '#454741', metalness: 0.7, roughness: 0.32 })
  const brushed = new THREE.MeshStandardMaterial({ color: '#a7a79e', metalness: 0.8, roughness: 0.27 })
  const stone = new THREE.MeshStandardMaterial({ color: '#b8b8ae', metalness: 0.12, roughness: 0.72 })
  const glass = new THREE.MeshStandardMaterial({ color: '#e7e5dc', metalness: 0.25, roughness: 0.22, transparent: true, opacity: 0.58 })
  const frame = new THREE.Group()
  doorAssembly.add(frame)
  makeBox(0.12, 4.2, 0.18, darkMetal, -1.25, 0, 0, frame)
  makeBox(0.12, 4.2, 0.18, darkMetal, 1.25, 0, 0, frame)
  makeBox(2.62, 0.12, 0.18, darkMetal, 0, 2.04, 0, frame)
  makeBox(2.62, 0.12, 0.18, darkMetal, 0, -2.04, 0, frame)
  makeBox(2.28, 3.84, 0.09, stone, 0, 0, -0.06, frame)

  const leaf = makeBox(2.12, 3.68, 0.16, brushed, 0, 0, 0.15)
  parts.push({ mesh: leaf, base: leaf.position.clone(), offset: 0.72 })
  const face = makeBox(1.96, 3.5, 0.045, stone, 0, 0, 0.25)
  parts.push({ mesh: face, base: face.position.clone(), offset: 1.02 })
  const inset = makeBox(1.68, 3.18, 0.025, glass, 0, 0, 0.29)
  parts.push({ mesh: inset, base: inset.position.clone(), offset: 1.35 })
  const inner = makeBox(1.52, 3.02, 0.022, brushed, 0, 0, 0.32)
  parts.push({ mesh: inner, base: inner.position.clone(), offset: 1.68 })
  for (let i = 0; i < 5; i++) {
    const hinge = makeBox(0.11, 0.3, 0.09, darkMetal, -1.12, 1.25 - i * 0.62, 0.25)
    parts.push({ mesh: hinge, base: hinge.position.clone(), offset: 0.42 })
  }
  const handle = makeBox(0.055, 0.52, 0.09, darkMetal, 0.76, 0, 0.37)
  parts.push({ mesh: handle, base: handle.position.clone(), offset: 0.58 })
  const escutcheon = makeBox(0.18, 0.72, 0.04, brushed, 0.76, 0, 0.35)
  parts.push({ mesh: escutcheon, base: escutcheon.position.clone(), offset: 0.9 })

  const drafting = new THREE.LineBasicMaterial({ color: '#77786f', transparent: true, opacity: 0.8 })
  makeLine([[-1.7, -2.4, 0], [1.7, -2.4, 0]], drafting)
  makeLine([[-1.7, 2.35, 0], [1.7, 2.35, 0]], drafting)
  makeLine([[-1.58, -2.2, 0], [-1.58, 2.2, 0]], drafting)
  makeLine([[1.58, -2.2, 0], [1.58, 2.2, 0]], drafting)
  for (let i = 0; i < 18; i++) {
    const y = -2.25 + i * 0.26
    makeLine([[-1.68, y, 0], [-1.57, y, 0]], drafting)
    makeLine([[1.57, y, 0], [1.68, y, 0]], drafting)
  }

  const gridMaterial = new THREE.LineBasicMaterial({ color: '#a1a198', transparent: true, opacity: 0.45 })
  const gridPoints = []
  for (let i = -10; i <= 10; i++) {
    gridPoints.push([i * 0.35, -2.5, -1.8], [i * 0.35, -2.5, 1.8])
    gridPoints.push([-3.5, -2.5, i * 0.18], [3.5, -2.5, i * 0.18])
  }
  for (let i = 0; i < gridPoints.length; i += 2) makeLine([gridPoints[i], gridPoints[i + 1]], gridMaterial)

  resize()
  observer = new ResizeObserver(resize)
  observer.observe(host.value)
  ready.value = true
  const animate = () => {
    animationFrameId = requestAnimationFrame(animate)
    currentRotationY += (targetRotationY - currentRotationY) * 0.08
    currentRotationX += (targetRotationX - currentRotationX) * 0.08
    root.rotation.y = currentRotationY
    root.rotation.x = currentRotationX
    renderer.render(scene, camera)
  }
  animate()
})
onUnmounted(() => {
  cancelAnimationFrame(animationFrameId)
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
  <section class="door-study" aria-labelledby="door-study-title">
    <div class="door-study-heading">
      <div><span class="door-eyebrow">TECHNICAL STUDY / 001</span><h3 id="door-study-title">Complete skeleton<br /><em>door design.</em></h3></div>
      <p>A mechanism considered layer by layer.<br />Drag to inspect the assembly in real time.</p>
    </div>
    <div class="door-stage">
      <div ref="host" class="door-canvas" @pointerdown="onPointerDown" @pointermove="onPointerMove" @pointerup="onPointerUp" @pointercancel="onPointerUp" @lostpointercapture="onPointerUp" aria-label="Interactive 3D architectural door model. Drag to rotate." role="img"></div>
      <div class="door-stage-top"><span>FIG. 001 / ASSEMBLY</span><span>3D · LIVE MODEL</span></div>
      <div class="door-stage-bottom"><span>01 — FRAME / LEAF / HARDWARE</span><span>ROTATE ↔</span></div>
      <div v-if="!ready" class="door-loading">Preparing technical model…</div>
    </div>
    <div class="door-controls">
      <span>EXPLODED VIEW</span>
      <button :aria-pressed="exploded" @click="setExploded(!exploded)">{{ exploded ? 'Reassemble' : 'Explode assembly' }} <span>{{ exploded ? '−' : '+' }}</span></button>
      <button @click="targetRotationY = -0.28; targetRotationX = -0.08">Reset view <span>↺</span></button>
    </div>
  </section>
</template>
