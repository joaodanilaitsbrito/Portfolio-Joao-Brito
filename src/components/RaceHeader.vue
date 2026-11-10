<script setup>
import { Menu, X } from 'lucide-vue-next'
defineProps({ menuOpen: Boolean })
defineEmits(['toggle', 'navigate'])
const links = [
  ['01', 'Perfil', '#profile'],
  ['02', 'Trajetória', '#experience'],
  ['03', 'Projetos', '#projects'],
  ['04', 'Contato', '#contact']
]
</script>

<template>
  <header class="site-header">
    <a class="brand" href="#home" @click="$emit('navigate')">
      <span class="brand-mark">JP<span>///</span></span>
      <span class="brand-copy">AUTOMOBILISMO<br><small>PORTFÓLIO / 2026</small></span>
    </a>
    <button class="menu-toggle" @click="$emit('toggle')" :aria-label="menuOpen ? 'Fechar menu' : 'Abrir menu'">
      <X v-if="menuOpen" :size="22" /><Menu v-else :size="22" />
    </button>
    <nav :class="{ 'nav-open': menuOpen }">
      <a v-for="link in links" :key="link[0]" :href="link[2]" @click="$emit('navigate')">
        <span>{{ link[0] }}</span>{{ link[1] }}
      </a>
      <a class="nav-cta" href="#contact" @click="$emit('navigate')">VAMOS CONVERSAR <span>↗</span></a>
    </nav>
  </header>
</template>
