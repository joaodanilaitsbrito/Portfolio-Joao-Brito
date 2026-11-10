<script setup>
import { ArrowUpRight, Crosshair, Cpu, Flag } from 'lucide-vue-next'
const traits = [
  { icon: Cpu, title: 'TECNOLOGIA', text: 'Interesse por desenvolvimento, sistemas e soluções digitais.' },
  { icon: Crosshair, title: 'PRECISÃO', text: 'Atenção aos detalhes, organização e melhoria contínua.' },
  { icon: Flag, title: 'EVOLUÇÃO', text: 'Aprender, testar, corrigir a rota e buscar o próximo nível.' }
]
</script>
<template>
  <section id="profile" class="content-section section-wrap">
    <div class="section-heading">
      <div><span class="section-index">01 / PERFIL</span><h2>MAIS QUE<br><em>VELOCIDADE.</em></h2></div>
      <p class="section-side-note">A mentalidade de quem busca<br>sempre a próxima evolução.</p>
    </div>
    <div class="profile-layout">
      <div class="profile-copy">
        <span class="eyebrow">QUEM ESTÁ AO VOLANTE?</span>
        <h3>Sou João Pedro.<br><span>Curioso por natureza.</span></h3>
        <p>Sou estudante de Sistemas de Informação e profissional com experiências nas áreas administrativa e de qualidade. Minha trajetória combina organização, visão prática e interesse por tecnologia.</p>
        <p>O automobilismo representa algo que levo também para minha vida profissional: estratégia, concentração, consistência e a vontade de melhorar a cada volta.</p>
        <a class="inline-link" href="#contact">VAMOS CONVERSAR <ArrowUpRight :size="16" /></a>
      </div>
      <div class="trait-grid">
        <article v-for="(trait, index) in traits" :key="trait.title" class="trait-card">
          <div class="trait-top"><component :is="trait.icon" :size="21" /><span>0{{ index + 1 }}</span></div>
          <h4>{{ trait.title }}</h4><p>{{ trait.text }}</p>
          <div class="trait-line"></div>
        </article>
      </div>
    </div>
    <div class="quote-band"><span>“</span><p>O resultado é construído volta após volta.<br><strong>Na pista e fora dela.</strong></p><small>— MINHA MENTALIDADE</small></div>
  </section>
</template>
