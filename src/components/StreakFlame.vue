<template>
  <span class="flame-container" :style="{ width: size + 'px', height: size + 'px' }">
    <svg 
      class="streak-flame" 
      viewBox="0 0 24 24" 
      xmlns="http://www.w3.org/2000/svg"
      :style="{ width: size + 'px', height: size + 'px' }"
    >
      <defs>
        <linearGradient :id="'flame-grad-' + id" x1="0%" y1="100%" x2="0%" y2="0%">
          <stop offset="0%" stop-color="#EA580C" />
          <stop offset="50%" stop-color="#F97316" />
          <stop offset="100%" stop-color="#FBBF24" />
        </linearGradient>
        <linearGradient :id="'flame-inner-grad-' + id" x1="0%" y1="100%" x2="0%" y2="0%">
          <stop offset="0%" stop-color="#F97316" />
          <stop offset="70%" stop-color="#FBBF24" />
          <stop offset="100%" stop-color="#FEF08A" />
        </linearGradient>
        <filter :id="'flame-glow-' + id" x="-20%" y="-20%" width="140%" height="140%">
          <feGaussianBlur stdDeviation="1" result="blur" />
          <feComposite in="SourceGraphic" in2="blur" operator="over" />
        </filter>
      </defs>
      
      <!-- Outer Flame Glow -->
      <path 
        class="flame-layer flame-outer" 
        :fill="'url(#flame-grad-' + id + ')'" 
        :filter="'url(#flame-glow-' + id + ')'"
        d="M17.65,10c-1.46-4.52-5.65-7.7-5.65-7.7s-1,1.05-1,2.54a6.45,6.45,0,0,0,3,5.43c1.7,1.07,2.15,3,1.4,4.86a4.29,4.29,0,0,1-4.14,2.83,4.45,4.45,0,0,1-4.45-4.45c0-2.42,1.38-4.52,3.4-5.59C7.81,9.08,6,12.33,6,16a6,6,0,0,0,12,0C18,13.8,17.85,11.9,17.65,10Z"
      />
      
      <!-- Inner Flame -->
      <path 
        class="flame-layer flame-inner" 
        :fill="'url(#flame-inner-grad-' + id + ')'"
        d="M15.5,12.5c-.7-2-2.7-3.5-2.7-3.5s-.5.5-.5,1.2a3,3,0,0,0,1.4,2.5c.8.5,1,1.4.7,2.2a2,2,0,0,1-1.9,1.3,2,2,0,0,1-2-2c0-1.1.6-2,1.5-2.5c-1.6.4-2.5,1.9-2.5,3.6a2.7,2.7,0,0,0,5.4,0C15.6,14.3,15.6,13.4,15.5,12.5Z"
        transform="scale(0.85) translate(2, 2.5)"
      />
      
      <!-- Center Bright Core -->
      <path 
        class="flame-layer flame-core" 
        fill="#FFFFFF"
        opacity="0.85"
        d="M12,13.5c-.2-.6-.8-1-.8-1s-.2.1-.2.4a1,1,0,0,0,.4.8c.2.2.3.4.2.7a.6.6,0,0,1-.6.4.6.6,0,0,1-.6-.6c0-.3.2-.6.5-.8c-.5.1-.8.6-.8,1.1a.9.9,0,0,0,1.8,0C12,14.1,12,13.8,12,13.5Z"
        transform="scale(0.7) translate(5, 7.5)"
      />
    </svg>
  </span>
</template>

<script setup>
import { ref } from 'vue'

defineProps({
  size: {
    type: [Number, String],
    default: 20
  }
})

const id = ref(Math.random().toString(36).substring(2, 9))
</script>

<style scoped>
.flame-container {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  vertical-align: middle;
  line-height: 0;
}

.streak-flame {
  transform-origin: center bottom;
  filter: drop-shadow(0 2px 6px rgba(234, 88, 12, 0.25));
}

.flame-layer {
  transform-origin: center bottom;
}

.flame-outer {
  animation: outer-flicker 1.8s ease-in-out infinite alternate;
}

.flame-inner {
  animation: inner-flicker 1.2s ease-in-out infinite alternate-reverse;
}

.flame-core {
  animation: core-flicker 0.8s ease-in-out infinite alternate;
}

@keyframes outer-flicker {
  0% {
    transform: scale(0.95) rotate(-2deg);
  }
  100% {
    transform: scale(1.05) rotate(2deg);
  }
}

@keyframes inner-flicker {
  0% {
    transform: scale(0.85) translate(2px, 2.5px) rotate(3deg);
  }
  100% {
    transform: scale(0.95) translate(2px, 2.5px) rotate(-3deg);
  }
}

@keyframes core-flicker {
  0% {
    transform: scale(0.7) translate(5px, 7.5px) scaleY(0.9);
    opacity: 0.7;
  }
  100% {
    transform: scale(0.7) translate(5px, 7.5px) scaleY(1.1);
    opacity: 0.95;
  }
}
</style>
