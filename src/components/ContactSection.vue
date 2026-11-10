<script setup>
import { ArrowUpRight, Mail, Phone, Github, MessageCircle } from 'lucide-vue-next'
const contacts = [
  { icon: Mail, label: 'E-MAIL', value: 'joaodanilaitsbrito@gmail.com', href: 'mailto:joaodanilaitsbrito@gmail.com' },
  { icon: Phone, label: 'TELEFONE', value: '(34) 9 8895-1322', href: 'tel:+5534988951322' },
  { icon: Github, label: 'GITHUB', value: 'joaodanilaitsbrito', href: 'https://github.com/joaodanilaitsbrito' }
]
</script>
<template>
  <section id="contact" class="contact-section">
    <div class="section-wrap contact-wrap">
      <span class="section-index">04 / CONTATO</span>
      <div class="contact-title"><h2>PRONTO PARA<br><em>A PRÓXIMA VOLTA?</em></h2><span class="contact-signal">VAMOS NOS CONECTAR <ArrowUpRight :size="18"/></span></div>
      <div class="contact-bottom">
        <p>Novas oportunidades começam com uma conversa.<br>Entre em contato e vamos construir algo juntos.</p>
        <div class="contact-list">
          <a v-for="item in contacts" :key="item.label" :href="item.href" target="_blank" rel="noreferrer" class="contact-item">
            <component :is="item.icon" :size="19"/><span><small>{{ item.label }}</small><strong>{{ item.value }}</strong></span><ArrowUpRight :size="16"/>
          </a>
        </div>
      </div>
      <a class="whatsapp-float" href="https://wa.me/5534988951322" target="_blank" rel="noreferrer"><MessageCircle :size="18"/> FALAR PELO WHATSAPP</a>
    </div>
  </section>
</template>
