<script setup>
import { onMounted, onBeforeUnmount, ref } from 'vue'
import { gsap } from 'gsap'

const wipeA = ref(null)
const wipeB = ref(null)
const cursor = ref(null)
const label = ref('')
const boost = { v: 0 }
const offs = []
const SFX = { profile: 'VRUUM!', experience: 'SKRRT!', split: 'BOOM!', projects: 'ZOOM!', contact: 'VAMOS!' }
const POW = ['POW!', 'BAM!', 'ZAP!', 'WHAM!']
const root = document.documentElement
const reduce = window.matchMedia('(prefers-reduced-motion: reduce)').matches
const fine = window.matchMedia('(pointer:fine)').matches

let raf, lastY = 0, v = 0, skew = 0, tracks = [], io, busy = false, moveCam

function loop() {
  const y = window.scrollY, dy = y - lastY
  lastY = y
  v += (Math.min(Math.abs(dy) / 70, 1) - v) * 0.12
  skew += (Math.max(-1, Math.min(1, dy / 60)) * -2.2 - skew) * 0.15
  root.style.setProperty('--speed', Math.max(v, boost.v).toFixed(3))
  root.style.setProperty('--skew', skew.toFixed(2) + 'deg')
  tracks.forEach((t) => {
    const dir = +t.dataset.dir
    t.style.transform = `translate3d(${dir > 0 ? -y * 0.6 : y * 0.6 - 3200}px,0,0)`
  })
  raf = requestAnimationFrame(loop)
}

function sfx(text) {
  const el = document.createElement('span')
  el.className = 'sfx'
  el.textContent = text
  el.style.top = `${15 + Math.random() * 50}%`
  el.style[Math.random() > 0.5 ? 'left' : 'right'] = '4%'
  document.body.appendChild(el)
  const r = gsap.utils.random(-14, 14)
  gsap.timeline({ onComplete: () => el.remove() })
    .fromTo(el, { scale: 3, opacity: 0, rotation: r * 2 }, { scale: 1, opacity: 1, rotation: r, duration: 0.28, ease: 'back.out(3)' })
    .to(el, { x: () => gsap.utils.random(-8, 8), duration: 0.05, repeat: 5, yoyo: true })
    .to(el, { opacity: 0, scale: 0.8, duration: 0.25, delay: 0.2 })
}

function burst(x, y) {
  const el = document.createElement('span')
  el.className = 'burst'
  el.textContent = POW[Math.floor(Math.random() * POW.length)]
  el.style.left = `${x - 75}px`
  el.style.top = `${y - 75}px`
  document.body.appendChild(el)
  gsap.timeline({ onComplete: () => el.remove() })
    .fromTo(el, { scale: 0, rotation: gsap.utils.random(-40, 40) }, { scale: 1, rotation: gsap.utils.random(-12, 12), duration: 0.22, ease: 'back.out(4)' })
    .to(el, { opacity: 0, scale: 1.4, duration: 0.25, delay: 0.25 })
}

function wipeTo(target) {
  busy = true
  const layers = [wipeB.value, wipeA.value]
  gsap.timeline({ onComplete: () => (busy = false) })
    .fromTo(layers, { xPercent: -120 }, { xPercent: 0, duration: 0.38, stagger: 0.07, ease: 'power3.in' })
    .add(() => target.scrollIntoView({ behavior: 'instant' }))
    .to(layers, { xPercent: 120, duration: 0.4, stagger: 0.07, ease: 'power3.out' })
}

function onClick(e) {
  const link = e.target.closest('a[href^="#"]')
  if (link) {
    const target = document.querySelector(link.getAttribute('href'))
    if (!target || reduce) return
    e.preventDefault()
    if (!busy) wipeTo(target)
    return
  }
  if (!reduce && !e.target.closest('button, input, textarea')) burst(e.clientX, e.clientY)
}

onMounted(() => {
  tracks = [...document.querySelectorAll('.marquee-track')]
  gsap.set([wipeA.value, wipeB.value], { skewX: -18, xPercent: -120 })
  if (reduce) return
  lastY = window.scrollY
  loop()
  document.addEventListener('click', onClick, true)
  const on = (t, e, f, o) => { t.addEventListener(e, f, o); offs.push(() => t.removeEventListener(e, f, o)) }
  document.querySelector('#home')?.setAttribute('data-cursor', 'SEGURE')
  if (fine) {
    const qx = gsap.quickTo(cursor.value, 'x', { duration: 0.18, ease: 'power3.out' })
    const qy = gsap.quickTo(cursor.value, 'y', { duration: 0.18, ease: 'power3.out' })
    let mag = null
    on(window, 'pointermove', (e) => {
      qx(e.clientX); qy(e.clientY)
      const t = e.target.closest?.('a,button,[data-cursor]')
      label.value = t?.dataset?.cursor || ''
      cursor.value.classList.toggle('hot', !!t)
      cursor.value.classList.toggle('lbl', !!label.value)
      const m = e.target.closest?.('.button,.nav-cta,.contact-item,.whatsapp-float')
      if (mag && mag !== m) gsap.to(mag, { x: 0, y: 0, duration: 0.6, ease: 'elastic.out(1,.4)' })
      mag = m
      if (m) { const r = m.getBoundingClientRect(); gsap.to(m, { x: (e.clientX - r.left - r.width / 2) * 0.25, y: (e.clientY - r.top - r.height / 2) * 0.35, duration: 0.3 }) }
    }, { passive: true })
  }
  const base = fine ? 1.06 : 1
  on(document.querySelector('#home'), 'pointerdown', (e) => {
    if (e.target.closest('a,button')) return
    gsap.to(boost, { v: 1, duration: 0.7 })
    gsap.to('.hero-photo-wrap', { scale: 1.18, duration: 0.9, ease: 'power2.out' })
  })
  on(window, 'pointerup', () => {
    if (boost.v === 0) return
    gsap.to(boost, { v: 0, duration: 0.8 })
    gsap.to('.hero-photo-wrap', { scale: base, duration: 1, ease: 'power3.out' })
  })

  io = new IntersectionObserver((entries) => {
    entries.forEach((en) => {
      if (en.isIntersecting && SFX[en.target.id]) { sfx(SFX[en.target.id]); io.unobserve(en.target) }
    })
  }, { threshold: 0.4 })
  Object.keys(SFX).forEach((id) => { const s = document.getElementById(id); if (s) io.observe(s) })

  const wrap = document.querySelector('.hero-photo-wrap')
  if (wrap && fine) {
    gsap.set(wrap, { scale: 1.06 })
    const qx = gsap.quickTo(wrap, 'x', { duration: 0.8, ease: 'power3.out' })
    const qy = gsap.quickTo(wrap, 'y', { duration: 0.8, ease: 'power3.out' })
    const qr = gsap.quickTo(wrap, 'rotation', { duration: 1, ease: 'power3.out' })
    moveCam = (e) => {
      const dx = e.clientX / window.innerWidth - 0.5, dy = e.clientY / window.innerHeight - 0.5
      qx(dx * -26); qy(dy * -18); qr(dx * 1.2)
    }
    window.addEventListener('pointermove', moveCam, { passive: true })
  }
})

onBeforeUnmount(() => {
  cancelAnimationFrame(raf)
  io?.disconnect()
  offs.forEach((f) => f())
  document.removeEventListener('click', onClick, true)
  if (moveCam) window.removeEventListener('pointermove', moveCam)
})
</script>

<template>
  <div class="comic-layer" aria-hidden="true">
    <div class="speedlines"></div>
    <div class="halftone"></div>
    <div ref="cursor" class="cursor"><span>{{ label }}</span></div>
    <div ref="wipeB" class="wipe wipe-black"></div>
    <div ref="wipeA" class="wipe wipe-orange"></div>
  </div>
</template>
