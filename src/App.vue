<template>
  <div class="page">
    <div
      class="overlay"
      :style="{
        gridTemplateColumns: `repeat(${GRID_COLS}, 1fr)`,
        gridTemplateRows:    `repeat(${GRID_ROWS}, 1fr)`,
      }"
    >
      <div v-for="cell in cells" :key="cell.id" class="grid-cell">
        <img
          v-if="cell.hasFlag"
          :src="danishFlag"
          class="flag"
          :style="{
            transform: `rotate(${cell.rot}deg)`,
            marginLeft: `${cell.offsetX}%`,
            marginTop:  `${cell.offsetY}%`,
          }"
          aria-hidden="true"
        />
      </div>
    </div>
    <h1 class="heading">Kære Mor,<br>Stort tillykke med fødselsdagen</h1>
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
import danishFlag from './assets/danish-flag.png'
import podimoLogo from './assets/logos/podimo-logo.png'
import taenkLogo from './assets/logos/taenk-logo.png'
import netflixLogo from './assets/logos/netflix.png'
import primeLogo from './assets/logos/prime.png'
import hboLogo from './assets/logos/hbo.png'

const gifts = [
  {
    id: 1,
    label: 'Gift 1',
    hueRotate: -160,
    saturate: 2.2,
    logo: podimoLogo,
    title: 'Mofibo - 12 måneder',
    message: 'Ubegrænset adgang til alle dine yndlingbøger og podcasts. God fornøjelse 😊<br> Hjertelig tillykke med fødselsdagen! 🎂',
  },
  {
    id: 2,
    label: 'Gift 2',
    hueRotate: -90,
    saturate: 2.0,
    logo: taenkLogo,
    title: 'Forbrugerrådet tænk & Streamingtjeneste',
    message: "Tænk medlemskab 12 måneder inkl. medlemsblad og valgfri streamingtjeneste i 6 måneder!<br>Kæmpe tillykke med fødselsdagen! 🎉",
  },
  {
    id: 3,
    label: 'Gift 3',
    hueRotate: 0,
    saturate: 1.4,
    logo: [netflixLogo, primeLogo, hboLogo],
    title: 'Streamingtjeneste - 12 måneder',
    message: 'Valgfri streamingtjeneste i 12 måneder! <br>Gå ikke glip af næste sæson af din ynglingsserie 📺 <br> Stort tillykke med fødselsdagen! 🎉',
  },
]

// ============================================================
// FLAG GRID — tweak these to get the look you want
// ============================================================
const GRID_COLS     = 9      // number of columns
const GRID_ROWS     = 7      // number of rows
const FLAG_SIZE     = 70     // px — width of each flag
const FLAG_OPACITY  = 0.15   // 0 (invisible) → 1 (fully visible)
const MAX_ROTATION  = 30     // max degrees clockwise / anti-clockwise
const MAX_OFFSET    = 30     // % of cell — how far flag can wander from center

// cell indices to leave empty: row * GRID_COLS + col (zero-based)
const skipCells = new Set([
  20, 21, 22, 23, 24,
  29, 30, 31, 32, 33,
  38, 39, 40, 41, 42,
])
// ============================================================

const rng = (s) => ((Math.sin(s) * 43758.5453) % 1 + 1) % 1

const cells = Array.from({ length: GRID_COLS * GRID_ROWS }, (_, i) => ({
  id:      i,
  hasFlag: !skipCells.has(i),
  rot:     rng(i)       * MAX_ROTATION * 2 - MAX_ROTATION,
  offsetX: rng(i + 200) * MAX_OFFSET   * 2 - MAX_OFFSET,
  offsetY: rng(i + 400) * MAX_OFFSET   * 2 - MAX_OFFSET,
}))

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
  position: relative;
  min-height: 100vh;
  background: linear-gradient(160deg, #c0392b 0%, #8e1a12 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 2rem;
  gap: 5rem;
}

.overlay {
  position: absolute;
  display: grid;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
}

.grid-cell {
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.flag {
  width: v-bind(FLAG_SIZE + 'px');
  height: auto;
  opacity: v-bind(FLAG_OPACITY);
  pointer-events: none;
  user-select: none;
  flex-shrink: 0;
}

.heading {
  color: #fff;
  font-size: clamp(2rem, 6vw, 4rem);
  text-shadow: 0 2px 12px rgba(0, 0, 0, 0.4);
  letter-spacing: 0.04em;
  text-align: center;
  position: relative;
  z-index: 1;
}

.gifts-row {
  display: flex;
  flex-wrap: wrap;
  gap: 4.5rem;
  justify-content: center;
  align-items: flex-end;
  position: relative;
  z-index: 1;
}
</style>
