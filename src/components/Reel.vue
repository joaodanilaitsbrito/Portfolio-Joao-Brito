<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import DecryptedText from './vuebits/DecryptedText.vue'
gsap.registerPlugin(ScrollTrigger)

const items = [
  { kind: 'EXPERIÊNCIA PROFISSIONAL', title: 'Auxiliar Administrativo', place: 'Supermercados PIC', city: 'Divinópolis / MG', text: 'Atuação em rotinas administrativas, organização de informações e suporte às atividades operacionais.' },
  { kind: 'JOVEM APRENDIZ', title: 'Aprendiz Administrativo', place: 'Farmax', city: 'Divinópolis / MG', text: 'Vivência nas áreas de Manutenção, Segurança e Qualidade, com apoio a planilhas e transferência de dados.' },
  { kind: 'ENSINO TÉCNICO', title: 'Técnico em Eletrotécnica', place: 'SENAI', city: 'Divinópolis / MG', text: 'Técnico de Nível Médio em Eletrotécnica.', status: 'CONCLUÍDO' },
  { kind: 'FORMAÇÃO TÉCNICA', title: 'Montagem e Manutenção de Computadores', place: 'SENAC', city: 'Divinópolis / MG', text: 'Fundamentos de hardware, montagem e manutenção.', status: 'CONCLUÍDO' },
  { kind: 'ENSINO SUPERIOR', title: 'Sistemas de Informação', place: 'Faculdade Anhanguera', city: 'Divinópolis / MG', text: 'Desenvolvimento web, sistemas, tecnologia e automação.', status: 'EM FORMAÇÃO' }
]
const root = ref(null), scroller = ref(null), track = ref(null), bar = ref(null)
let mm
onMounted(() => {
  mm = gsap.matchMedia()
  mm.add('(min-width: 901px) and (prefers-reduced-motion: no-preference)', () => {
    gsap.set(scroller.value, { overflow: 'visible' })
    const dist = () => Math.max(0, track.value.scrollWidth - window.innerWidth)
    gsap.to(track.value, {
      x: () => -dist(), ease: 'none',
      scrollTrigger: { trigger: root.value, start: 'top top', end: () => '+=' + dist(), pin: true, scrub: 0.8, anticipatePin: 1, invalidateOnRefresh: true,
        onUpdate: (s) => { bar.value.style.width = s.progress * 100 + '%' } }
    })
  })
})
onBeforeUnmount(() => mm?.revert())
</script>
<template>
  <section id="experience" ref="root" class="reel">
    <div class="reel-head">
      <div>
        <span class="section-index">02 / TRAJETÓRIA</span>
        <h2><DecryptedText text="TRAJETÓRIA" animate-on="view" :speed="45" :max-iterations="14" sequential /><br><em>EM MOVIMENTO.</em></h2>
      </div>
      <p class="section-side-note">Role para avançar.<br>Cada painel, uma nova volta.</p>
    </div>
    <div class="reel-bar"><i ref="bar"></i></div>
    <div ref="scroller" class="reel-scroll">
      <div ref="track" class="reel-track" data-cursor="ROLE">
        <article class="reel-panel photo">
          <img src="/kart-joao-pedro.webp" alt="João Pedro pilotando seu kart número 32" loading="lazy" />
          <div class="photo-cap"><small>LARGADA</small><strong>Kart nº 32</strong><span>Divinópolis / MG</span></div>
        </article>
        <article v-for="(it, i) in items" :key="it.title" class="reel-panel">
          <span class="reel-num">0{{ i + 1 }}</span>
          <small>{{ it.kind }}</small>
          <h3>{{ it.title }}</h3>
          <h4>{{ it.place }}</h4>
          <p>{{ it.text }}</p>
          <div class="reel-tag">{{ it.city }}<template v-if="it.status"> · {{ it.status }}</template></div>
        </article>
      </div>
    </div>
  </section>
</template>
