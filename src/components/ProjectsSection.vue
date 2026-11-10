<script setup>
import { Github, ArrowUpRight, Code2, Layers3, MonitorSmartphone } from 'lucide-vue-next'
const projects = [
  { number: '01', tag: 'DESENVOLVIMENTO WEB', title: 'Projetos acadêmicos', description: 'Aplicações e atividades desenvolvidas durante a graduação em Sistemas de Informação.', icon: Code2, stack: 'HTML / CSS / JAVASCRIPT' },
  { number: '02', tag: 'SISTEMAS E DADOS', title: 'Soluções digitais', description: 'Estudos e protótipos envolvendo organização de dados, sistemas e ferramentas digitais.', icon: Layers3, stack: 'BANCO DE DADOS / LÓGICA' },
  { number: '03', tag: 'PRÁTICA E TECNOLOGIA', title: 'Hardware & tecnologia', description: 'Conhecimentos de montagem, manutenção de computadores e fundamentos de eletrotécnica.', icon: MonitorSmartphone, stack: 'HARDWARE / ELETRÔNICA' }
]
</script>
<template>
  <section id="projects" class="content-section projects-section">
    <div class="section-wrap">
      <div class="section-heading">
        <div><span class="section-index">03 / PROJETOS</span><h2>IDEIAS QUE<br><em>SAEM DO PAPEL.</em></h2></div>
        <p class="section-side-note">Um espaço para projetos,<br>experimentos e aprendizado.</p>
      </div>
      <div class="projects-grid">
        <article v-for="project in projects" :key="project.number" class="project-card">
          <div class="project-top"><span>{{ project.number }} / PROJECT</span><component :is="project.icon" :size="22" /></div>
          <div class="project-graphic"><div class="graphic-cross"></div><span>{{ project.number }}</span><div class="graphic-orbit"></div></div>
          <small>{{ project.tag }}</small><h3>{{ project.title }}</h3><p>{{ project.description }}</p>
          <div class="project-stack">{{ project.stack }}</div>
        </article>
      </div>
      <div class="github-callout">
        <div><Github :size="28"/><div><strong>ACOMPANHE MINHA EVOLUÇÃO</strong><span>Confira meus códigos e repositórios no GitHub.</span></div></div>
        <a href="https://github.com/joaodanilaitsbrito" target="_blank" rel="noreferrer" class="button button-outline">ACESSAR GITHUB <ArrowUpRight :size="16"/></a>
      </div>
    </div>
  </section>
</template>
