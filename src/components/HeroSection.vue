<template>
  <div class="hero-section">
    <transition name="pop">
      <div class="speech-bubble" v-show="showBubble">
        <h2>
          {{ typed1 }}<br v-if="typed1.length === text1.length" />
          <span class="highlight">{{ typed2 }}</span>{{ typed3 }}
          <span class="cursor" v-if="isTyping">|</span>
        </h2>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const showBubble = ref(false)
const isTyping = ref(false)

const variations = [
  { text1: 'Hora de', text2: 'conversar', text3: ' com o mundo!' },
  { text1: 'Pronto para', text2: 'praticar', text3: ' hoje?' },
  { text1: 'Vamos', text2: 'aprender', text3: ' um novo idioma?' },
  { text1: 'Seu próximo', text2: 'desafio', text3: ' começa agora!' },
  { text1: 'Cada palavra é um', text2: 'passo', text3: ' a mais!' }
]

const text1 = ref('')
const text2 = ref('')
const text3 = ref('')

const typed1 = ref('')
const typed2 = ref('')
const typed3 = ref('')

onMounted(async () => {
  const selected = variations[Math.floor(Math.random() * variations.length)]
  text1.value = selected.text1
  text2.value = selected.text2
  text3.value = selected.text3
  
  // Little delay before bubble appears
  setTimeout(() => {
    showBubble.value = true
    isTyping.value = true
    startTyping()
  }, 500)
})

const startTyping = async () => {
  const type = async (source, targetRef, delay = 50) => {
    for (let i = 0; i < source.length; i++) {
      targetRef.value += source[i]
      await new Promise(r => setTimeout(r, delay))
    }
  }

  await type(text1.value, typed1)
  await type(text2.value, typed2)
  await type(text3.value, typed3)
  
  isTyping.value = false
}
</script>

<style scoped>
.hero-section {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding-top: 100px;
  padding-left: 32px;
  position: relative;
  flex: 1;
}

.speech-bubble {
  background: white;
  padding: 16px 24px;
  border-radius: 24px;
  border-bottom-right-radius: 4px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
  margin-bottom: -20px;
  position: relative;
  z-index: 2;
  text-align: center;
}

.speech-bubble::after {
  content: '';
  position: absolute;
  bottom: -12px;
  right: 24px;
  border-width: 12px 0 0 16px;
  border-style: solid;
  border-color: white transparent transparent transparent;
}

.speech-bubble h2 {
  font-size: 20px;
  font-weight: 700;
  color: var(--text-dark);
  line-height: 1.3;
}

.highlight {
  color: var(--primary-blue);
}

.mascot-container {
  width: 100%;
  max-width: 300px;
  display: flex;
  justify-content: center;
  position: relative;
  z-index: 1;
}

.mascot-image {
  width: 100%;
  height: auto;
  object-fit: contain;
  filter: drop-shadow(0 10px 15px rgba(0,0,0,0.1));
}
</style>
