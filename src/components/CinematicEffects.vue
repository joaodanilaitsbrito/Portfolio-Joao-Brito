<script setup>
import { onMounted, onBeforeUnmount } from 'vue'

let canvas, ctx, frameId, resizeHandler, moveHandler, leaveHandler
let particles = []
let pointer = { x: -1000, y: -1000 }
let width = 0
let height = 0
let reducedMotion = false

function resize() {
  if (!canvas) return
  const ratio = Math.min(window.devicePixelRatio || 1, 1.5)
  width = window.innerWidth
  height = window.innerHeight
  canvas.width = width * ratio
  canvas.height = height * ratio
  ctx.setTransform(ratio, 0, 0, ratio, 0, 0)
  const count = window.innerWidth < 700 ? 22 : 48
  particles = Array.from({ length: count }, () => ({
    x: Math.random() * width,
    y: Math.random() * height,
    vx: (Math.random() - .5) * .35,
    vy: (Math.random() - .5) * .35,
    radius: Math.random() * 1.8 + .5,
    alpha: Math.random() * .45 + .12
  }))
}

function animate() {
  if (!ctx) return
  ctx.clearRect(0, 0, width, height)
  particles.forEach((p) => {
    p.x += p.vx
    p.y += p.vy
    if (p.x < -10) p.x = width + 10
    if (p.x > width + 10) p.x = -10
    if (p.y < -10) p.y = height + 10
    if (p.y > height + 10) p.y = -10

    const dx = pointer.x - p.x
    const dy = pointer.y - p.y
    const distance = Math.hypot(dx, dy)
    if (distance < 115 && distance > 0) {
      p.x -= dx / distance * .45
      p.y -= dy / distance * .45
    }

    ctx.beginPath()
    ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2)
    ctx.fillStyle = `rgba(255, 105, 45, ${p.alpha})`
    ctx.fill()
  })
  frameId = requestAnimationFrame(animate)
}

function handlePointer(event) {
  pointer.x = event.clientX
  pointer.y = event.clientY
  document.documentElement.style.setProperty('--cursor-x', `${event.clientX}px`)
  document.documentElement.style.setProperty('--cursor-y', `${event.clientY}px`)
}

function handleLeave() {
  pointer.x = -1000
  pointer.y = -1000
}

function handleTilt(event) {
  if (reducedMotion || !window.matchMedia('(pointer:fine)').matches) return
  const card = event.target.closest('.trait-card, .education-card, .project-card, .timeline-card')
  if (!card) return
  const rect = card.getBoundingClientRect()
  const x = (event.clientX - rect.left) / rect.width
  const y = (event.clientY - rect.top) / rect.height
  card.style.setProperty('--tilt-x', `${(0.5 - y) * 8}deg`)
  card.style.setProperty('--tilt-y', `${(x - 0.5) * 10}deg`)
  card.style.setProperty('--spot-x', `${x * 100}%`)
  card.style.setProperty('--spot-y', `${y * 100}%`)
  card.classList.add('card-emphasis')
}

function resetTilt(event) {
  const card = event.target.closest('.trait-card, .education-card, .project-card, .timeline-card')
  if (!card) return
  card.style.setProperty('--tilt-x', '0deg')
  card.style.setProperty('--tilt-y', '0deg')
  card.classList.remove('card-emphasis')
}

onMounted(() => {
  reducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
  canvas = document.querySelector('#efeitos-canvas')
  ctx = canvas?.getContext('2d')
  resizeHandler = resize
  moveHandler = handlePointer
  leaveHandler = handleLeave
  resize()
  if (!reducedMotion) {
    animate()
    window.addEventListener('pointermove', moveHandler, { passive: true })
    window.addEventListener('pointerout', leaveHandler)
    document.addEventListener('pointermove', handleTilt, { passive: true })
    document.addEventListener('pointerleave', resetTilt)
  }
  window.addEventListener('resize', resizeHandler, { passive: true })
})

onBeforeUnmount(() => {
  cancelAnimationFrame(frameId)
  window.removeEventListener('pointermove', moveHandler)
  window.removeEventListener('pointerout', leaveHandler)
  window.removeEventListener('resize', resizeHandler)
  document.removeEventListener('pointermove', handleTilt)
  document.removeEventListener('pointerleave', resetTilt)
})
</script>

<template>
  <div class="camada-efeitos" aria-hidden="true">
    <canvas id="efeitos-canvas"></canvas>
    <div class="brilho-cursor"></div>
    <div class="grade-cinematografica"></div>
  </div>
</template>
