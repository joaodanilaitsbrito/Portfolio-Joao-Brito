<script setup>
import { ref, onMounted } from 'vue'
const items = [
  { w: 'NA PISTA', t: 'Estratégia, concentração e consistência. A mentalidade de quem busca melhorar a cada volta.', tags: ['KART Nº 32', 'FOCO', 'CONSISTÊNCIA'] },
  { w: 'NO CÓDIGO', t: 'Sistemas, web e hardware. Aprender, testar, corrigir a rota e buscar o próximo nível.', tags: ['SISTEMAS', 'WEB', 'HARDWARE'] }
]
const hover = ref(0)
const locked = ref(null)
const sig = ref(null)
const active = (i) => (locked.value !== null ? locked.value === i : hover.value === i)
const over = (i) => { if (locked.value === null) hover.value = i }
const toggle = (i) => { locked.value = locked.value === i ? null : i; hover.value = i }
onMounted(() => {
  const io = new IntersectionObserver(([e]) => { if (e.isIntersecting) { sig.value.classList.add('drawn'); io.disconnect() } }, { threshold: 0.6 })
  io.observe(sig.value)
})
</script>
<template>
  <section id="split" class="split-section">
    <div class="section-wrap">
      <span class="section-index">DOIS MODOS / UMA MENTALIDADE</span>
      <div class="split">
        <button v-for="(it, i) in items" :key="it.w" class="split-half" :class="{ active: active(i) }" data-cursor="TRAVAR"
          :aria-pressed="locked === i" @mouseenter="over(i)" @focus="over(i)" @click="toggle(i)">
          <span v-if="locked === i" class="lock">● TRAVADO</span>
          <h3>{{ it.w }}</h3>
          <p>{{ it.t }}</p>
          <div class="tags"><span v-for="t in it.tags" :key="t">{{ t }}</span></div>
        </button>
      </div>
      <p class="split-hint">PASSE O MOUSE · CLIQUE PARA TRAVAR</p>
      <svg ref="sig" class="signature" viewBox="0 0 440 120" aria-label="Assinatura João Pedro"><text x="10" y="88">João Pedro</text></svg>
    </div>
  </section>
</template>
