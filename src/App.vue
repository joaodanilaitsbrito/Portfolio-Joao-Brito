<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { gsap } from 'gsap'
import RaceHeader from './components/RaceHeader.vue'
import HeroSection from './components/HeroSection.vue'
import TelemetryBar from './components/TelemetryBar.vue'
import ProfileSection from './components/ProfileSection.vue'
import ProjectsSection from './components/ProjectsSection.vue'
import ContactSection from './components/ContactSection.vue'
import SiteFooter from './components/SiteFooter.vue'
import CinematicEffects from './components/CinematicEffects.vue'
import Preloader from './components/Preloader.vue'
import Reel from './components/Reel.vue'
import TrackSplit from './components/TrackSplit.vue'
import ComicFX from './components/ComicFX.vue'
import Marquee from './components/Marquee.vue'

const scrollProgress = ref(0)
const menuOpen = ref(false)

function updateProgress() {
  const scrollable = document.documentElement.scrollHeight - window.innerHeight
  scrollProgress.value = scrollable > 0 ? (window.scrollY / scrollable) * 100 : 0
}

function closeMenu() { menuOpen.value = false }

onMounted(() => {
  updateProgress()
  window.addEventListener('scroll', updateProgress, { passive: true })
  gsap.from('.page-shell', { opacity: 0, duration: 1, ease: 'power2.out' })
})
onBeforeUnmount(() => window.removeEventListener('scroll', updateProgress))
</script>

<template>
  <div class="page-shell">
    <Preloader />
    <CinematicEffects />
    <ComicFX />
    <div class="scroll-progress" :style="{ width: `${scrollProgress}%` }"></div>
    <RaceHeader :menu-open="menuOpen" @toggle="menuOpen = !menuOpen" @navigate="closeMenu" />
    <main>
      <HeroSection />
      <TelemetryBar />
      <ProfileSection />
      <TrackSplit />
      <Marquee text="NO SEU PRÓPRIO RITMO" />
      <Reel />
      <Marquee text="TECNOLOGIA • DESEMPENHO • EVOLUÇÃO" alt reverse />
      <ProjectsSection />
      <ContactSection />
    </main>
    <SiteFooter />
    <a class="back-top" href="#home" aria-label="Voltar ao início">↑</a>
  </div>
</template>
