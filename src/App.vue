<template>
  <div class="page">
    <h1 class="heading">Kære Mor,<br>kæmpe tillykke med fødselsdagen</h1>
    <div class="gifts-row">
      <GiftBox
        v-for="gift in gifts"
        :key="gift.id"
        :hueRotate="gift.hueRotate"
        :saturate="gift.saturate"
        :isOpen="openedIds.has(gift.id)"
        :title="gift.title"
        :modalOpen="selected !== null"
        @open="openModal(gift)"
        @select="celebrate(gift)"
      />
    </div>
    <GiftModal :gift="selected" @close="selected = null" />
    <CelebrationModal :gift="celebrated" @close="celebrated = null" />
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
import GiftBox from './components/GiftBox.vue'
import GiftModal from './components/GiftModal.vue'
import CelebrationModal from './components/CelebrationModal.vue'
import podimoLogo from './assets/logos/podimo-logo.png'
import taenkLogo from './assets/logos/taenk-logo.png'
import netflixLogo from './assets/logos/netflix.png'
import primeLogo from './assets/logos/prime.png'
import hboLogo from './assets/logos/hbo.png'

const gifts = [
  {
    id: 1,
    label: 'Gift 1',
    hueRotate: -160,   // blue → gold/yellow box, violet bow
    saturate: 2.2,
    logo: podimoLogo,
    title: 'Podimo - 12 måneder',
    message: 'Ubegrænset adgang til alle dine yndlingbøger og podcasts. Hjertelig tillykke med fødselsdagen! 🎂',
  },
  {
    id: 2,
    label: 'Gift 2',
    hueRotate: -90,    // blue → green box, pink/magenta bow
    saturate: 2.0,
    logo: taenkLogo,
    title: 'Forbrugerrådet tænk + Streamingtjeneste',
    message: "Tænk medlemskab 12 måneder inkl. medlemsblad og valgfri streamingtjeneste i 6 måneder! 🎉",
  },
  {
    id: 3,
    label: 'Gift 3',
    hueRotate: 0,      // original blue-grey box, yellow bow
    saturate: 1.4,
    logo: [netflixLogo, primeLogo, hboLogo],
    title: 'Streamingtjeneste - 12 måneder',
    message: 'Valgfri streamingtjeneste i 12 måneder! Gå ikke glip af næste sæson af din ynglingsserie 📺',
  },
]

const openedIds = reactive(new Set())
const selected = ref(null)
const celebrated = ref(null)

function openModal(gift) {
  openedIds.add(gift.id)
  selected.value = gift
}

function celebrate(gift) {
  celebrated.value = gift
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  background: linear-gradient(160deg, #c0392b 0%, #8e1a12 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  gap: 5rem;
}

.heading {
  color: #fff;
  font-size: clamp(2rem, 6vw, 4rem);
  text-shadow: 0 2px 12px rgba(0, 0, 0, 0.4);
  letter-spacing: 0.04em;
  text-align: center;
}

.gifts-row {
  display: flex;
  flex-wrap: wrap;
  gap: 4.5rem;
  justify-content: center;
  align-items: flex-end;
}
</style>
