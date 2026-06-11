<template>
  <div class="language-selector">
    <button @click="toggleMenu" class="lang-btn">
      <img src="https://flagcdn.com/w20/br.png" alt="Brasil Flag" class="flag-icon" />
      <span>Português Brasil</span>
      <ChevronDownIcon size="16" />
    </button>
    <div v-if="isOpen" class="dropdown">
      <button v-for="lang in languages" :key="lang.code" @click="selectLanguage(lang)" class="lang-item">
        <img :src="lang.flag" alt="Flag" class="flag-icon-small" />
        {{ lang.label }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { ChevronDown as ChevronDownIcon } from '@lucide/vue'

const isOpen = ref(false)
const languages = [
  { code: 'pt', label: 'Português Brasil', flag: 'https://flagcdn.com/w20/br.png' },
  { code: 'en', label: 'English', flag: 'https://flagcdn.com/w20/us.png' },
  { code: 'fr', label: 'Français', flag: 'https://flagcdn.com/w20/fr.png' },
  { code: 'es', label: 'Español', flag: 'https://flagcdn.com/w20/es.png' }
]

const toggleMenu = () => {
  isOpen.value = !isOpen.value
}

const selectLanguage = (lang) => {
  isOpen.value = false
  // For now, only Portuguese is fully supported as requested
}
</script>

<style scoped>
.language-selector {
  position: absolute;
  top: 20px;
  right: 20px;
  z-index: 10;
}

.lang-btn {
  background: white;
  border: none;
  border-radius: 20px;
  padding: 8px 16px;
  display: flex;
  align-items: center;
  gap: 8px;
  color: var(--primary-blue);
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transition: transform 0.2s;
  cursor: pointer;
  font-weight: 600;
  font-size: 14px;
}

.lang-btn:active {
  transform: scale(0.95);
}

.flag-icon {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  object-fit: cover;
}

.flag-icon-small {
  width: 16px;
  height: 16px;
  border-radius: 50%;
  object-fit: cover;
  margin-right: 8px;
}

.dropdown {
  position: absolute;
  top: 50px;
  right: 0;
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  padding: 8px 0;
  min-width: 160px;
  display: flex;
  flex-direction: column;
}

.lang-item {
  background: none;
  border: none;
  padding: 10px 16px;
  display: flex;
  align-items: center;
  font-size: 14px;
  color: var(--text-dark);
}

.lang-item:hover {
  background: #f5f8ff;
  color: var(--primary-blue);
}
</style>
