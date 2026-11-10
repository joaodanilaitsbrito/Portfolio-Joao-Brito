<script setup>
defineProps({ text: String, alt: Boolean, reverse: Boolean })
</script>
<template>
  <div class="marquee" :class="{ alt }" aria-hidden="true">
    <div class="marquee-track" :data-dir="reverse ? -1 : 1">
      <span v-for="n in 16" :key="n">{{ text }} <b>///</b> </span>
    </div>
  </div>
</template>
