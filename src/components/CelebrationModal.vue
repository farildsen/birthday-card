<template>
  <Teleport to="body">
    <Transition name="celebrate">
      <div v-if="gift" class="overlay" @click.self="emit('close')">
        <div class="card" role="dialog" aria-modal="true">
          <button class="close-btn" @click="emit('close')" aria-label="Close">&times;</button>

          <div class="sparkles">🎉 🎊 🎂 🎊 🎉</div>

          <div v-if="gift.logo" class="logos">
            <template v-if="Array.isArray(gift.logo)">
              <img v-for="(src, i) in gift.logo" :key="i" :src="src" class="logo" :alt="gift.title" />
            </template>
            <img v-else :src="gift.logo" class="logo" :alt="gift.title" />
          </div>
          <div v-else class="icon">🎁</div>

          <h2>{{ gift.title }}</h2>
          <p>{{ gift.message }}</p>

          <div class="ribbon ribbon-left"></div>
          <div class="ribbon ribbon-right"></div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
import { watch, ref } from 'vue'
import confetti from 'canvas-confetti'

const props = defineProps({
  gift: { type: Object, default: null },
})

const emit = defineEmits(['close'])

const running = ref(false)
const colors = ['#c0392b', '#f1c40f', '#2ecc71', '#3498db', '#9b59b6', '#e67e22']

function frame() {
  if (!running.value) return
  confetti({ particleCount: 6, angle: 60,  spread: 70, origin: { x: 0, y: 0.6 }, colors })
  confetti({ particleCount: 6, angle: 120, spread: 70, origin: { x: 1, y: 0.6 }, colors })
  requestAnimationFrame(frame)
}

watch(() => props.gift, (val) => {
  if (val) {
    running.value = true
    requestAnimationFrame(frame)
  } else {
    running.value = false
  }
})
</script>

<style scoped>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.35);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 200;
  padding: 1rem;
}

.card {
  position: relative;
  background: linear-gradient(145deg, #fffdf7 0%, #fff5f5 100%);
  border-radius: 28px;
  padding: 4rem 4rem 3.5rem;
  max-width: 720px;
  width: 100%;
  text-align: center;
  box-shadow: 0 30px 80px rgba(0, 0, 0, 0.45), 0 0 0 4px #c0392b, 0 0 0 8px #f1c40f;
  overflow: hidden;
  animation: popIn 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
}

.close-btn {
  position: absolute;
  top: 1rem;
  right: 1.25rem;
  background: none;
  border: none;
  font-size: 2rem;
  line-height: 1;
  cursor: pointer;
  color: #aaa;
  transition: color 0.15s;
  z-index: 1;
}

.close-btn:hover {
  color: #333;
}

.sparkles {
  font-size: 2rem;
  margin-bottom: 1.5rem;
  letter-spacing: 0.5rem;
  animation: pulse 1.5s ease-in-out infinite;
}

.icon {
  font-size: 5rem;
  margin-bottom: 1.25rem;
}

.logos {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  margin-bottom: 1.75rem;
}

.logo {
  max-width: 160px;
  max-height: 90px;
  width: auto;
  height: auto;
  object-fit: contain;
}

h2 {
  font-size: 2.5rem;
  font-weight: 700;
  color: #8e1a12;
  margin-bottom: 1.25rem;
  text-shadow: 0 1px 4px rgba(142, 26, 18, 0.15);
}

p {
  font-size: 1.35rem;
  color: #444;
  line-height: 1.9;
  max-width: 560px;
  margin: 0 auto;
}

.ribbon {
  position: absolute;
  top: 0;
  width: 60px;
  height: 100%;
  opacity: 0.07;
  background: repeating-linear-gradient(
    180deg,
    #c0392b 0px,
    #c0392b 20px,
    #f1c40f 20px,
    #f1c40f 40px
  );
  pointer-events: none;
}

.ribbon-left  { left: 0; }
.ribbon-right { right: 0; }

@keyframes popIn {
  0%   { transform: scale(0.7); opacity: 0; }
  100% { transform: scale(1);   opacity: 1; }
}

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50%       { transform: scale(1.08); }
}

/* Transition */
.celebrate-enter-active,
.celebrate-leave-active {
  transition: opacity 0.3s ease;
}
.celebrate-enter-from,
.celebrate-leave-to {
  opacity: 0;
}
</style>
