<template>
  <Teleport to="body">
    <Transition name="modal">
      <div v-if="gift" class="overlay" @click.self="emit('close')">
        <div class="card" role="dialog" aria-modal="true">
          <button class="close-btn" @click="emit('close')" aria-label="Close">&times;</button>
          <div v-if="gift.logo" class="logos">
            <template v-if="Array.isArray(gift.logo)">
              <img v-for="(src, i) in gift.logo" :key="i" :src="src" class="logo" :alt="gift.title" />
            </template>
            <img v-else :src="gift.logo" class="logo" :alt="gift.title" />
          </div>
          <div v-else class="icon">🎁</div>
          <h2>{{ gift.title }}</h2>
          <p v-html="gift.message"></p>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
defineProps({
  gift: { type: Object, default: null },
})

const emit = defineEmits(['close'])
</script>

<style scoped>
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.65);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
  padding: 1rem;
}

.card {
  position: relative;
  background: #fffdf7;
  border-radius: 20px;
  padding: 3.5rem 3rem;
  max-width: 600px;
  width: 100%;
  text-align: center;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.35);
  animation: unblur 3s ease-in forwards;
}

.close-btn {
  position: absolute;
  top: 0.8rem;
  right: 1rem;
  background: none;
  border: none;
  font-size: 1.8rem;
  line-height: 1;
  cursor: pointer;
  color: #888;
  transition: color 0.15s;
}

.close-btn:hover {
  color: #333;
}

.icon {
  font-size: 4rem;
  margin-bottom: 1rem;
}

.logos {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.75rem;
  margin-bottom: 1.5rem;
}

.logo {
  max-width: 140px;
  max-height: 80px;
  width: auto;
  height: auto;
  object-fit: contain;
}

h2 {
  font-size: 2rem;
  color: #8e1a12;
  margin-bottom: 1.25rem;
}

p {
  font-size: 1.25rem;
  color: #444;
  line-height: 1.8;
}

/* Transition */
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.25s ease;
}

.modal-enter-active .card,
.modal-leave-active .card {
  transition: transform 0.25s ease, opacity 0.25s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .card,
.modal-leave-to .card {
  transform: scale(0.88);
  opacity: 0;
}

@keyframes unblur {
  0%   { filter: blur(20px); }
  100% { filter: blur(0px); }
}
</style>
