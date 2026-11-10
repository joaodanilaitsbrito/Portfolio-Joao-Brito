<script setup>
import { ArrowDown, ArrowUpRight } from 'lucide-vue-next'
import { onMounted, onBeforeUnmount } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
gsap.registerPlugin(ScrollTrigger)

let context
onMounted(() => {
  context = gsap.context(() => {
    const intro = gsap.timeline({ delay: window.__intro ? 1.9 : 0, defaults: { ease: 'power4.out' } })
    intro.from('.hero-photo', { scale: 1.22, duration: 1.8, ease: 'power2.out' })
      .from('.hero-photo-overlay', { opacity: 0, duration: 1.1 }, '-=1.3')
      .from('.hero-kicker', { y: 24, opacity: 0, duration: .7 }, '-=.8')
      .from('.hero-title-line', { yPercent: 115, opacity: 0, stagger: .13, duration: 1 }, '-=.45')
      .from('.hero-description, .hero-actions, .hero-stats', { y: 24, opacity: 0, stagger: .14, duration: .7 }, '-=.5')
      .from('.hero-photo-caption, .hero-side-data', { opacity: 0, x: 24, stagger: .15, duration: .8 }, '-=.9')

    gsap.to('.hero-photo', {
      yPercent: 10,
      ease: 'none',
      scrollTrigger: { trigger: '.hero', start: 'top top', end: 'bottom top', scrub: true }
    })
    gsap.utils.toArray('.content-section, .experience-section').forEach((section) => {
      gsap.from(section.querySelectorAll('.section-heading, .profile-copy, .trait-card, .timeline-card, .education-card, .project-card, .github-callout'), {
        y: 55, opacity: 0, duration: .85, stagger: .12, ease: 'power3.out',
        scrollTrigger: { trigger: section, start: 'top 78%', once: true }
      })
    })
  })
})
onBeforeUnmount(() => context?.revert())
</script>

<template>
  <section id="home" class="hero cinematic-hero">
    <div class="hero-photo-wrap">
      <img class="hero-photo" src="/kart-joao-pedro.webp" alt="João Pedro pilotando seu kart número 32" fetchpriority="high" />
      <div class="hero-photo-overlay"></div>
      <div class="hero-photo-grain"></div>
      <div class="hero-photo-caption"><span>JOÃO PEDRO</span><i></i><span>KART / PILOTO Nº 32</span></div>
      <div class="hero-side-data"><span>MODO PISTA</span><strong>32</strong><small>ACELERAÇÃO TOTAL<br>DIVINÓPOLIS / MG</small></div>
    </div>

    <div class="hero cinematic-content section-wrap">
      <div class="hero-kicker"><span class="live-dot"></span> PORTFÓLIO PESSOAL <span class="kicker-line"></span> DIVINÓPOLIS / MG</div>
      <div class="hero-title">
        <div class="title-overline">TECNOLOGIA • DESEMPENHO • EVOLUÇÃO</div>
        <div class="title-mask"><h1 class="hero-title-line">MOVIDO</h1></div>
        <div class="title-mask title-outline"><h1 class="hero-title-line">PELA PAIXÃO<span class="orange-dot">.</span></h1></div>
      </div>
      <p class="hero-description">Da pista ao universo da tecnologia: estratégia, precisão e vontade de evoluir em cada novo desafio.</p>
      <div class="hero-actions">
        <a class="button button-primary" href="#profile">EXPLORAR PORTFÓLIO <ArrowUpRight :size="17" /></a>
        <a class="text-link" href="#projects">VER PROJETOS <span>↗</span></a>
      </div>
      <div class="hero-stats">
        <div><strong>01<span>/</span></strong><small>FOCO EM<br>TECNOLOGIA</small></div>
        <div><strong>02<span>/</span></strong><small>MENTALIDADE<br>DE EVOLUÇÃO</small></div>
        <div><strong>32<span>/</span></strong><small>NÚMERO<br>NA PISTA</small></div>
      </div>
    </div>
    <a href="#profile" class="scroll-cue"><span><ArrowDown :size="14" /></span> ROLE PARA EXPLORAR</a>
    <div class="hero-watermark">NO SEU PRÓPRIO RITMO</div>
  </section>
</template>
