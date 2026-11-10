<script setup>
import { Gauge, Zap, Flag, Activity } from 'lucide-vue-next'
const items = [
  { icon: Gauge, label: 'MENTALIDADE', value: 'DESEMPENHO' },
  { icon: Zap, label: 'ENERGIA', value: 'SEMPRE EM MOVIMENTO' },
  { icon: Flag, label: 'LOCALIZAÇÃO', value: 'DIVINÓPOLIS / MG' },
  { icon: Activity, label: 'STATUS', value: 'EM DESENVOLVIMENTO' }
]
</script>
<template>
  <div class="telemetry-strip">
    <div class="telemetry-inner">
      <div v-for="item in items" :key="item.label" class="telemetry-item">
        <component :is="item.icon" :size="17" />
        <div><small>{{ item.label }}</small><strong>{{ item.value }}</strong></div>
      </div>
      <div class="telemetry-check">● SISTEMA ATIVO</div>
    </div>
  </div>
</template>
