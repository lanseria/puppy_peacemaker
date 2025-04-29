<script setup lang="ts">
import { computed, ref } from 'vue'

interface Step {
  img: string
  rejectText: string
}

// Add the default image as the very first step
const steps: Step[] = [
  { img: '/assets/imgs/default.png', rejectText: '对不起，小狗知道错了' }, // Start with default
  { img: '/assets/imgs/sorry.png', rejectText: '以后好吃的都给你' },
  { img: '/assets/imgs/please.png', rejectText: '你在玩欲擒故纵吗?' },
  { img: '/assets/imgs/ask.png', rejectText: '不许点这里' },
  { img: '/assets/imgs/angry.png', rejectText: '原谅小狗好不好' },
  // Note: The 'beg.png' is now the last *asking* step before the logic disables the button
  { img: '/assets/imgs/beg.png', rejectText: '真的不能原谅吗？' }, // Adjusted last text slightly
]

const lastStep = {
  img: '/assets/imgs/happy.png',
  content: '汪！狗狗就知道',
}

const currentStepIndex = ref(0)
const isForgiven = ref(false)
const forgiveBtnScale = ref(1)

const currentStep = computed(() => steps[currentStepIndex.value]!)
// Check against steps.length - 1 because index is 0-based
const isLastStep = computed(() => currentStepIndex.value >= steps.length - 1)

function onReject() {
  if (!isLastStep.value) {
    currentStepIndex.value++
    // Reduce scale increment and cap to prevent excessive size
    forgiveBtnScale.value = Math.min(forgiveBtnScale.value + 0.2, 2.5) // Increments by 0.2, max 2.5x size
  }
}

function onForgive() {
  isForgiven.value = true
}
</script>

<template>
  <div class="p-4 bg-[#f1d5da] flex flex-col min-h-screen items-center justify-center overflow-hidden">
    <!-- Vue Transition for overall state change -->
    <Transition name="fade" mode="out-in">
      <!-- Asking State -->
      <div v-if="!isForgiven" key="asking" class="text-center flex flex-col w-full items-center">
        <!-- Vue Transition for image changes -->
        <Transition name="fade" mode="out-in">
          <img
            :key="currentStep.img"
            :src="currentStep.img"
            alt="dog asking forgiveness"
            class="mb-6 rounded-2xl h-56 w-56 object-contain sm:h-64 sm:w-64"
          >
        </Transition>

        <div class="text-2xl text-[#68495b] font-bold mb-8">
          <!-- Increased margin-bottom slightly -->
          可以跟小狗和好吗？
        </div>

        <!-- Increased gap for better spacing, especially when scaling -->
        <div class="flex flex-col gap-6 max-w-xs w-full items-center justify-center sm:flex-row sm:gap-8 sm:max-w-sm">
          <button
            class="text-lg text-white font-bold px-8 py-3 rounded-lg border-none bg-[#d4818e] w-full transition-all duration-200 ease-in-out active:bg-[#b05c7e] hover:bg-[#c16a7a] sm:w-auto active:scale-95"
            :style="{ transform: `scale(${forgiveBtnScale})` }"
            style="transform-origin: center;"
            @click="onForgive"
          >
            和好
          </button>
          <button
            class="text-lg text-white font-bold px-8 py-3 rounded-lg border-none bg-[#6784b1] w-full transition-all duration-150 active:bg-[#46597a] hover:bg-[#536a90] disabled:(opacity-60 cursor-not-allowed) sm:w-auto active:scale-95"
            :disabled="isLastStep"
            @click="onReject"
          >
            {{ currentStep!.rejectText }} <!-- Use .rejectText -->
          </button>
        </div>
      </div>

      <!-- Forgiven State -->
      <div v-else key="forgiven" class="text-center flex flex-col items-center">
        <img
          :src="lastStep.img"
          alt="dog happy"
          class="animate-bounce-slow mb-6 rounded-2xl h-56 w-56 object-contain sm:h-64 sm:w-64"
        >
        <div class="text-3xl text-[#68495b] font-bold">
          {{ lastStep.content }}
        </div>
      </div>
    </Transition>
  </div>
</template>

<style scoped>
/* Fade Transition */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Ensure buttons have a minimum width */
button {
  min-width: 130px; /* Increased slightly */
}

/* Ensure scaling doesn't visually shift the button layout too much */
button[style*='scale'] {
  /* You could add perspective or will-change here if needed, but often not necessary */
}
</style>
