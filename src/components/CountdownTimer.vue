<template>
  <div v-show="showBox" id="countdown-timer-container" class="flex flex-center" style="width: 70vw; height: 70vh">
    <!-- eslint-disable-next-line vue/no-v-html -->
    <div v-show="showMessage" id="countdown-timer-message" style="position: absolute; font-size: 150px" v-html="messageText"></div>
    <div v-show="showCountdown && !showMessage" class="countdown-display text-secondary">
      <q-icon name="sym_o_arrow_upward" class="countdown-arrow" :style="{ color: arrowColor }" aria-hidden="true" />
      <div id="countdown-timer-value">{{ parseFloat(remainingSeconds.toFixed(0)) }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'

let intervalTimerId: number
const remainingSeconds = ref(0)
const startDuration = ref(0)

interface Props {
  duration: number
  messageDuration?: number
  messageText?: string
  arrowColor: string
}

const props = withDefaults(defineProps<Props>(), {
  messageDuration: 0.5,
  messageText: '😃',
})

const showBox = computed(() => {
  return remainingSeconds.value > 0
})
const showCountdown = computed(() => {
  return remainingSeconds.value > 0
})
const showMessage = computed(() => {
  return remainingSeconds.value <= props.messageDuration
})

onMounted(() => {
  startTimer()
})

onBeforeUnmount(() => {
  abortTimer()
})

const abortTimer = () => {
  clearInterval(intervalTimerId)
  remainingSeconds.value = 0
}
const startTimer = () => {
  startDuration.value = props.duration
  remainingSeconds.value = startDuration.value

  console.log(`starting timer, duration=${startDuration.value}`)

  intervalTimerId = window.setInterval(() => {
    remainingSeconds.value -= 0.05

    if (remainingSeconds.value <= 0) {
      clearInterval(intervalTimerId)
    }
  }, 50)
}
</script>

<style lang="css">
#countdown-timer-container {
  pointer-events: none;
  user-select: none;
}

.countdown-display {
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 15vh;
  font-weight: bold;
  line-height: 1;
  text-shadow: #222 0 0 10px;
}

.countdown-arrow {
  font-size: 60vh;
  animation: countdown-arrow-up 800ms ease-in-out infinite;
}

@keyframes countdown-arrow-up {
  0%,
  100% {
    transform: translateY(0);
    opacity: 0.65;
  }
  50% {
    transform: translateY(-2vh);
    opacity: 1;
  }
}
</style>
