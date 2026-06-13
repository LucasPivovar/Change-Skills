<template>
  <span class="map-container" :style="{ width: size + 'px', height: size + 'px' }">
    <svg 
      class="country-map-svg" 
      viewBox="0 0 24 24" 
      xmlns="http://www.w3.org/2000/svg"
      :style="{ width: size + 'px', height: size + 'px' }"
    >
      <defs>
        <!-- Brazil Clip -->
        <clipPath :id="'clip-br-' + id">
          <path d="M7,4.5 C9.5,3.5 13,3 15.5,4 C17.5,4.8 19,5.5 19.5,7 C20,8.5 21,10 21,11 C21,12 20,13.5 19,14.5 C18,15.5 16,15.5 14.5,16.5 C13,17.5 12,19.5 11.5,21.5 C11,21.5 10.5,19 9.5,17.5 C8.5,16 6,15.5 5,14.5 C4,13.5 3.5,11.5 4,10 C4.5,8.5 5.5,6.5 7,4.5 Z" />
        </clipPath>
        
        <!-- USA Clip -->
        <clipPath :id="'clip-us-' + id">
          <path d="M2.5,6.5 L21.5,6.5 L21.5,10.5 C21.5,11.5 20.8,12 21.2,13 C21.6,14 21,16 19.5,18 C18.5,18 18,16.5 17,16 L15,16 C14,16.8 13.5,18.5 12.5,18.5 C11.5,18.5 11,17 10,15.5 L8.5,15 C7,14.5 6,14.5 5,14 L3,14 L2,11.5 C2,9.5 2,8 2.5,6.5 Z" />
        </clipPath>

        <!-- France Clip -->
        <clipPath :id="'clip-fr-' + id">
          <path d="M12,3.5 C13,3.5 15.5,4 17.5,5.5 C18.5,6.5 17.5,9.5 18,11.5 C18.5,13.5 20,15 19.5,16.5 C19,18 16.5,18 15.5,18.5 L12,21.5 L8.5,18.5 C6,18.5 4,17 4,14 C4,11 6,10 6.5,8 C7,6 8,4.5 11,3.5 Z" />
        </clipPath>

        <!-- Spain Clip -->
        <clipPath :id="'clip-es-' + id">
          <path d="M3.5,6 L19.5,6 C20.5,7.5 21.5,10.5 20.5,13 C19.5,15.5 18,17.5 16,18 L12,19.5 L8.5,18 L8.5,13.5 L4.5,13.5 L4.5,8.5 Z" />
        </clipPath>
      </defs>

      <!-- Brazil (Português) -->
      <g v-if="normalizedLang === 'pt'" :clip-path="'url(#clip-br-' + id + ')'">
        <rect x="0" y="0" width="24" height="24" fill="#009739" />
        <polygon points="12,7 18.5,12 12,17 5.5,12" fill="#FCD116" />
        <circle cx="12" cy="12" r="2.8" fill="#002277" />
        <path d="M 8.8,12.2 C 10.4,11.3 12,11.3 13.8,12.2" stroke="white" stroke-width="0.6" fill="none" />
      </g>

      <!-- USA (Inglês) -->
      <g v-else-if="normalizedLang === 'en'" :clip-path="'url(#clip-us-' + id + ')'">
        <rect x="0" y="0" width="24" height="24" fill="#FFFFFF" />
        <!-- Stripes -->
        <rect x="0" y="6.5" width="24" height="1" fill="#B22234" />
        <rect x="0" y="8.5" width="24" height="1" fill="#B22234" />
        <rect x="0" y="10.5" width="24" height="1" fill="#B22234" />
        <rect x="0" y="12.5" width="24" height="1" fill="#B22234" />
        <rect x="0" y="14.5" width="24" height="1" fill="#B22234" />
        <rect x="0" y="16.5" width="24" height="1" fill="#B22234" />
        <rect x="0" y="18.5" width="24" height="1" fill="#B22234" />
        <!-- Blue canton -->
        <rect x="0" y="6.5" width="10" height="6" fill="#3C3B6E" />
        <!-- Stylized stars circle -->
        <circle cx="5" cy="9.5" r="2.2" stroke="white" stroke-width="0.6" stroke-dasharray="1 1.2" fill="none" />
      </g>

      <!-- France (Francês) -->
      <g v-else-if="normalizedLang === 'fr'" :clip-path="'url(#clip-fr-' + id + ')'">
        <rect x="0" y="0" width="9.3" height="24" fill="#002395" />
        <rect x="9.3" y="0" width="5.4" height="24" fill="#FFFFFF" />
        <rect x="14.7" y="0" width="9.3" height="24" fill="#ED2939" />
      </g>

      <!-- Spain (Espanhol) -->
      <g v-else-if="normalizedLang === 'es'" :clip-path="'url(#clip-es-' + id + ')'">
        <rect x="0" y="0" width="24" height="8" fill="#AD1519" />
        <rect x="0" y="8" width="24" height="8" fill="#FCD116" />
        <rect x="0" y="16" width="24" height="8" fill="#AD1519" />
        <!-- Stylized Shield -->
        <circle cx="8" cy="12" r="1.8" fill="#AD1519" />
        <circle cx="8" cy="12" r="0.8" fill="#FCD116" />
      </g>
    </svg>
  </span>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  language: {
    type: String,
    default: 'Português'
  },
  size: {
    type: [Number, String],
    default: 24
  }
})

const id = ref(Math.random().toString(36).substring(2, 9))

const normalizedLang = computed(() => {
  const lang = props.language.toLowerCase()
  if (lang.includes('ing') || lang.includes('en')) return 'en'
  if (lang.includes('esp') || lang.includes('es')) return 'es'
  if (lang.includes('fra') || lang.includes('fr')) return 'fr'
  return 'pt' // default
})
</script>

<style scoped>
.map-container {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  vertical-align: middle;
  line-height: 0;
}

.country-map-svg {
  filter: drop-shadow(0 1px 2px rgba(0,0,0,0.15));
  transition: transform 0.2s;
}

.map-container:hover .country-map-svg {
  transform: scale(1.1);
}
</style>
