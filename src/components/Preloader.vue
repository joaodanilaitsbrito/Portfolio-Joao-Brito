<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
let seen = false
try { seen = sessionStorage.getItem('jp-intro') === '1' } catch {}
const reduce = window.matchMedia('(prefers-reduced-motion: reduce)').matches
const show = ref(!seen && !reduce)
window.__intro = show.value
const n = ref(0)
const el = ref(null)
onMounted(() => {
  if (!show.value) return
  document.body.style.overflow = 'hidden'
  const o = { v: 0 }
  gsap.timeline()
    .to(o, { v: 100, duration: 1.5, ease: 'power2.inOut', onUpdate: () => (n.value = Math.round(o.v)) })
    .to(el.value, { yPercent: -100, duration: 0.8, ease: 'power4.inOut', delay: 0.15 })
    .add(() => {
      document.body.style.overflow = ''
      show.value = false
      try { sessionStorage.setItem('jp-intro', '1') } catch {}
      ScrollTrigger.refresh()
    })
})
</script>
<template>
  <div v-if="show" ref="el" class="preloader" aria-hidden="true">
    <div class="pre-top">JP<span>///</span></div>
    <div class="pre-count">{{ n }}<small>%</small></div>
    <div class="pre-foot"><div class="pre-label">IGNIÇÃO · CARREGANDO PORTFÓLIO</div><div class="pre-bar"><i :style="{ width: n + '%' }"></i></div></div>
  </div>
</template>
