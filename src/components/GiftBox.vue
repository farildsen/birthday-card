<template>
  <div class="gift-outer">
    <div
      class="gift-wrap"
      :class="{ opening }"
      :style="{ '--hue': hueRotate + 'deg', '--sat': saturate }"
      @click="handleClick"
      role="button"
      tabindex="0"
      @keydown.enter="handleClick"
    >
      <Transition name="flip" mode="out-in">
        <img v-if="!isOpen" key="wrapped"   class="gift-img" :src="wrappedUrl"   alt="wrapped gift" />
        <img v-else         key="unwrapped" class="gift-img" :src="unwrappedUrl" alt="unwrapped gift" />
      </Transition>
    </div>
    <Transition name="label">
      <div v-if="isOpen && !modalOpen" class="gift-label">
        <span class="gift-title">{{ title }}</span>
        <button class="vaelg-btn" @click.stop="emit('select')">Vælg</button>
      </div>
    </Transition>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import wrappedUrl   from '../assets/gift-wrapped.svg'
import unwrappedUrl from '../assets/gift-unwrapped.svg'

defineProps({
  isOpen:    { type: Boolean, default: false },
  hueRotate: { type: Number,  default: 0 },
  saturate:  { type: Number,  default: 1 },
  title:     { type: String,  default: '' },
  modalOpen: { type: Boolean, default: false },
})

const emit = defineEmits(['open', 'select'])
const opening = ref(false)

function handleClick() {
  if (opening.value) return
  opening.value = true
  setTimeout(() => {
    emit('open')
    opening.value = false
  }, 1400)
}
</script>

<style scoped>
.gift-outer {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.gift-wrap {
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: pointer;
  transition: transform 0.2s ease;
  user-select: none;
  outline: none;
}

.gift-wrap:hover {
  transform: scale(1.1) translateY(-6px);
}

.gift-wrap:focus-visible {
  outline: 3px solid rgba(255, 255, 255, 0.7);
  border-radius: 8px;
}

.gift-img {
  width: 230px;
  height: auto;
  display: block;
  filter: hue-rotate(var(--hue)) saturate(var(--sat)) drop-shadow(0 8px 20px rgba(0, 0, 0, 0.35));
}

.gift-wrap.opening .gift-img {
  animation: shake 1.4s ease forwards;
}

@keyframes shake {
  0%   { transform: rotate(0deg) scale(1); }
  8%   { transform: rotate(-7deg) scale(1.05); }
  16%  { transform: rotate(7deg) scale(1.06); }
  24%  { transform: rotate(-9deg) scale(1.09); }
  32%  { transform: rotate(9deg) scale(1.09); }
  40%  { transform: rotate(-7deg) scale(1.07); }
  48%  { transform: rotate(7deg) scale(1.07); }
  56%  { transform: rotate(-9deg) scale(1.09); }
  64%  { transform: rotate(9deg) scale(1.09); }
  72%  { transform: rotate(-5deg) scale(1.04); }
  82%  { transform: rotate(5deg) scale(1.04); }
  91%  { transform: rotate(-2deg) scale(1.01); }
  100% { transform: rotate(0deg) scale(1); }
}

.gift-label {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  margin-top: 3rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  gap: 0.75rem;
  background: #fffdf7;
  border-radius: 16px;
  padding: 1rem 1.5rem;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.25);
  width: 230px;
  height: 110px;
  z-index: 10;
}

.gift-title {
  color: #8e1a12;
  font-size: 0.95rem;
  font-weight: 700;
  text-align: center;
  word-break: break-word;
  overflow-wrap: break-word;
}

.vaelg-btn {
  background: #8e1a12;
  color: #fff;
  border: none;
  border-radius: 20px;
  padding: 0.4rem 1.4rem;
  font-size: 0.95rem;
  font-weight: 700;
  cursor: pointer;
  font-family: inherit;
  transition: background 0.15s;
}

.vaelg-btn:hover {
  background: #c0392b;
}

.label-enter-active,
.label-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}
.label-enter-from,
.label-leave-to {
  opacity: 0;
  transform: translateY(6px);
}

.flip-enter-active,
.flip-leave-active {
  transition: opacity 0.18s ease;
}
.flip-enter-from,
.flip-leave-to {
  opacity: 0;
}
</style>
