<template>
  <div class="chat-container">
    <!-- Header -->
    <header class="chat-header">
      <button class="back-btn" @click="$emit('goBack')">
        <ChevronLeftIcon size="24" stroke-width="2.5" />
      </button>
      <div class="user-info">
        <img src="https://i.pravatar.cc/150?img=47" alt="Ana Silva" class="avatar" />
        <span class="user-name">Ana Silva</span>
      </div>
      <div class="timer-badge" :class="{ 'warning': timeRemaining <= 60 }" v-if="!isSaved">
        <ClockIcon size="16" class="timer-icon" />
        <span class="timer-text">{{ formattedTime }}</span>
      </div>
    </header>

    <div class="objective-wrapper">
      <!-- Mission Box -->
      <div class="mission-box" :class="{ 'is-open': isMissionOpen }">
        <div class="mission-header" @click="toggleMission">
          <div class="header-left">
            <FlagIcon size="18" class="mission-icon" />
            <span class="mission-title"><strong>Missão:</strong> Praticar e descobrir interesses</span>
          </div>
          <ChevronDownIcon size="20" class="toggle-icon" :class="{ 'rotated': isMissionOpen }" />
        </div>
        
        <div class="mission-content" v-show="isMissionOpen">
          <p class="mission-desc">Converse com seu parceiro e descubra mais sobre os interesses dele.</p>
          
          <div class="mission-icons">
            <div class="mission-item" :class="{ 'completed': objectives.hobbies }">
              <div class="icon-circle"><TargetIcon size="20" /></div>
              <span>Hobbies</span>
            </div>
            <div class="mission-item" :class="{ 'completed': objectives.music }">
              <div class="icon-circle"><TargetIcon size="20" /></div>
              <span>Música</span>
            </div>
            <div class="mission-item" :class="{ 'completed': objectives.movies }">
              <div class="icon-circle"><TargetIcon size="20" /></div>
              <span>Filmes</span>
            </div>
            <div class="mission-item" :class="{ 'completed': objectives.sports }">
              <div class="icon-circle"><TargetIcon size="20" /></div>
              <span>Esportes</span>
            </div>
          </div>
        </div>
        
        <img src="../assets/dica-hero.png" class="mission-mascot" />
        
        <!-- Success Banner -->
        <div class="success-banner" v-if="showSuccessBanner">
          <span>🎉 Missão Concluída!</span>
        </div>
      </div>
    </div>

    <!-- Content -->
    <div class="chat-content" ref="messagesListRef">
      <!-- Messages Area -->
      <div class="messages-list">
        <div 
          v-for="(msg, index) in messages" 
          :key="index"
          :class="['message-row', msg.isMine ? 'mine' : 'theirs', msg.isSuggestion ? 'suggestion-row' : '']"
          :style="{ animationDelay: (index * 0.1) + 's' }"
        >
          <img v-if="!msg.isMine && !msg.isSuggestion" src="https://i.pravatar.cc/150?img=47" class="msg-avatar" />
          
          <div v-if="msg.isSuggestion" class="suggestion-card">
            <img src="../assets/dica-hero.png" class="suggestion-mascot" />
            <div class="suggestion-content">
              <span class="suggestion-title">Sugestão de pergunta</span>
              <p>{{ msg.text }}</p>
            </div>
            <ChevronRightIcon size="20" class="suggestion-arrow" />
          </div>
          
          <div v-else class="message-bubble" :class="msg.isMine ? 'bubble-mine' : 'bubble-theirs'">
            <p>{{ msg.text }}</p>
            <span class="message-time" v-if="msg.time">
              {{ msg.time }}
              <CheckCheckIcon size="12" class="read-receipt" v-if="msg.isMine" />
            </span>
          </div>
        </div>
      </div>
    </div>

    <!-- Input Footer -->
    <div class="chat-input-area">
      <div class="input-wrapper">
        <input type="text" placeholder="Digite sua mensagem..." v-model="newMessage" @keyup.enter="sendMessage" />
        <button class="send-btn" @click="sendMessage" :disabled="!newMessage.trim()">
          <SendIcon size="18" stroke-width="2.5" />
        </button>
      </div>
    </div>

    <!-- End Conversation Modal -->
    <div class="end-modal-backdrop" v-if="showEndModal">
      <div class="end-modal-content">
        <h2>Tempo Esgotado!</h2>
        <p v-if="objectiveCompleted">Parabéns! Você completou o objetivo da conversa.</p>
        <p v-else>Que tal dar mais tempo ou buscar uma nova conversa?</p>
        
        <div class="end-actions">
          <button class="primary-btn" @click="$emit('continueChat')">Mandar solicitação</button>
          <button class="secondary-btn" @click="$emit('newPartner')">Procurar novo parceiro</button>
          <button class="outline-btn" @click="$emit('goBack')">Voltar para cursos</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, nextTick, watch } from 'vue'
import { 
  ChevronLeftIcon, 
  ChevronDownIcon,
  ClockIcon, 
  TargetIcon, 
  FlagIcon,
  SendIcon, 
  ChevronRightIcon,
  CheckCheckIcon
} from '@lucide/vue'

const isMissionOpen = ref(false)

const toggleMission = () => {
  isMissionOpen.value = !isMissionOpen.value
}

const props = defineProps({
  isSaved: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['goBack', 'continueChat', 'newPartner'])

const timeRemaining = ref(5 * 60)
const showEndModal = ref(false)
let timerInterval = null
const messagesListRef = ref(null)

const showSuccessBanner = ref(false)

const formattedTime = computed(() => {
  const minutes = Math.floor(timeRemaining.value / 60)
  const seconds = timeRemaining.value % 60
  return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`
})

const startTimer = () => {
  if (props.isSaved) return; // No timer if saved chat
  
  timerInterval = setInterval(() => {
    if (timeRemaining.value > 0) {
      timeRemaining.value--
    } else {
      clearInterval(timerInterval)
      showEndModal.value = true
    }
  }, 1000)
}

onMounted(() => {
  startTimer()
  scrollToBottom()
})

onUnmounted(() => {
  if (timerInterval) clearInterval(timerInterval)
})

const objectives = ref({
  hobbies: true,
  music: false,
  movies: false,
  sports: false
})

const objectiveCompleted = computed(() => {
  return objectives.value.hobbies && objectives.value.music && objectives.value.movies && objectives.value.sports
})

watch(objectiveCompleted, (newVal) => {
  if (newVal) {
    showSuccessBanner.value = true
    setTimeout(() => {
      showSuccessBanner.value = false
    }, 3000)
  }
})

const messages = ref([
  { text: 'Hi! How are you doing?', time: '10:30', isMine: true },
  { text: 'I\'m great! How about you?', time: '10:30', isMine: false },
  { text: 'I\'m good too! What do you like to do in your free time?', time: '10:31', isMine: true },
  { text: 'I love listening to music. It relaxes me a lot.', time: '10:31', isMine: false },
  { text: 'What\'s your favorite kind of music?', time: '', isMine: false, isSuggestion: true },
  { text: 'Nice! What kind of music?', time: '10:32', isMine: true },
  { text: 'I love pop and indie!', time: '10:32', isMine: false }
])

const newMessage = ref('')

const scrollToBottom = () => {
  nextTick(() => {
    if (messagesListRef.value) {
      messagesListRef.value.scrollTop = messagesListRef.value.scrollHeight
    }
  })
}

const sendMessage = () => {
  if (newMessage.value.trim() === '') return
  
  messages.value.push({
    text: newMessage.value,
    time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
    isMine: true
  })
  newMessage.value = ''
  scrollToBottom()
  
  // Simulate auto completion
  if (!objectives.value.music) {
    objectives.value.music = true
  } else if (!objectives.value.movies) {
    objectives.value.movies = true
  } else if (!objectives.value.sports) {
    objectives.value.sports = true
  }
}
</script>

<style scoped>
.chat-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #f4f8ff;
  z-index: 60;
  display: flex;
  flex-direction: column;
  animation: slideIn 0.3s ease;
}

@keyframes slideIn {
  from { transform: translateX(100%); }
  to { transform: translateX(0); }
}

.chat-header {
  display: flex;
  align-items: center;
  padding: 16px 20px;
  background: white;
  border-bottom: 1px solid #e2e8f0;
}

.back-btn {
  background: none;
  border: none;
  color: #1c5bf0;
  cursor: pointer;
  padding: 8px;
  margin-left: -8px;
}

.user-info {
  display: flex;
  align-items: center;
  flex: 1;
  margin-left: 12px;
}

.avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  object-fit: cover;
  margin-right: 12px;
}

.user-name {
  font-size: 16px;
  font-weight: 800;
  color: #1a235c;
}

.timer-badge {
  display: flex;
  align-items: center;
  gap: 6px;
  background: white;
  border: 1px solid #c7d2fe;
  padding: 6px 12px;
  border-radius: 20px;
  color: #1c5bf0;
}

.timer-badge.warning {
  border-color: #fca5a5;
  color: #ef4444;
}

.timer-text {
  font-weight: 700;
  font-size: 14px;
}

.objective-wrapper {
  padding: 20px 20px 0 20px;
  background: #f4f8ff;
  z-index: 10;
  flex-shrink: 0;
}

.chat-content {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
  display: flex;
  flex-direction: column;
}

.mission-box {
  background: white;
  border-radius: 20px;
  padding: 16px;
  position: relative;
  box-shadow: 0 4px 16px rgba(34, 197, 94, 0.08);
  border: 1px solid #dcfce7;
}

.mission-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  color: #16a34a;
  font-size: 14px;
  margin-bottom: 4px;
  cursor: pointer;
  position: relative;
  z-index: 5;
}

.header-left {
  display: flex;
  align-items: center;
  gap: 8px;
}

.toggle-icon {
  transition: transform 0.3s ease;
}

.toggle-icon.rotated {
  transform: rotate(180deg);
}

.mission-icon {
  fill: #16a34a;
}

.mission-title strong {
  font-weight: 800;
}

.mission-content {
  margin-top: 8px;
}

.mission-desc {
  font-size: 12px;
  color: #64748b;
  margin: 0 0 16px 0;
  max-width: 70%;
}

.mission-icons {
  display: flex;
  justify-content: space-between;
  border-top: 1px solid #f1f5f9;
  padding-top: 12px;
}

.mission-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: 6px;
  flex: 1;
  color: #94a3b8;
}

.mission-item.completed {
  color: #16a34a;
}

.icon-circle {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: #f8fafc;
  display: flex;
  align-items: center;
  justify-content: center;
  color: inherit;
}

.mission-item.completed .icon-circle {
  background: #dcfce7;
}

.mission-item span {
  font-size: 10px;
  font-weight: 700;
  line-height: 1.2;
}

.mission-mascot {
  position: absolute;
  right: -10px;
  top: -15px;
  width: 80px;
  height: 80px;
  object-fit: contain;
  pointer-events: none;
}

.success-banner {
  position: absolute;
  bottom: -10px;
  left: 50%;
  transform: translateX(-50%);
  background: #22c55e;
  color: white;
  padding: 6px 16px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 800;
  white-space: nowrap;
  box-shadow: 0 4px 12px rgba(34, 197, 94, 0.3);
  animation: slideUpFade 0.3s ease;
  z-index: 10;
}

.messages-list {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

@keyframes slideUpFade {
  from { opacity: 0; transform: translateY(15px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes slideInRight {
  from { opacity: 0; transform: translateX(40px); }
  to { opacity: 1; transform: translateX(0); }
}

@keyframes slideInLeft {
  from { opacity: 0; transform: translateX(-40px); }
  to { opacity: 1; transform: translateX(0); }
}

.message-row {
  display: flex;
  align-items: flex-end;
  gap: 8px;
  margin-bottom: 4px;
  opacity: 0;
}

.message-row.mine {
  justify-content: flex-end;
  animation: slideInRight 0.45s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
}

.message-row.theirs {
  justify-content: flex-start;
  animation: slideInLeft 0.45s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
}

.message-row.suggestion-row {
  margin: 12px 0;
  justify-content: center;
}

.msg-avatar {
  width: 28px;
  height: 28px;
  border-radius: 50%;
  margin-bottom: 8px;
}

.message-bubble {
  max-width: 75%;
  padding: 12px 16px;
  position: relative;
  word-break: break-word;
  overflow-wrap: break-word;
}

.message-bubble p {
  margin: 0 0 4px 0;
  font-size: 14px;
  line-height: 1.4;
  font-weight: 500;
}

.message-time {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 4px;
  font-size: 10px;
  font-weight: 600;
  text-align: right;
  opacity: 0.7;
}

.bubble-theirs {
  background: #e2e8f0;
  color: #1e293b;
  border-radius: 20px 20px 20px 4px;
}

.bubble-mine {
  background: #e0e7ff;
  color: #1c5bf0;
  border-radius: 20px 20px 4px 20px;
}

.suggestion-card {
  display: flex;
  align-items: center;
  background: white;
  border-radius: 20px;
  padding: 12px 16px;
  box-shadow: 0 4px 12px rgba(28, 91, 240, 0.08);
  border: 1px solid #e0e7ff;
  max-width: 90%;
}

.suggestion-mascot {
  width: 40px;
  height: 40px;
  object-fit: contain;
  margin-right: 12px;
}

.suggestion-content {
  flex: 1;
}

.suggestion-title {
  font-size: 11px;
  color: #1c5bf0;
  font-weight: 700;
  display: block;
  margin-bottom: 2px;
}

.suggestion-content p {
  margin: 0;
  font-size: 14px;
  color: #1a235c;
  font-weight: 700;
}

.suggestion-arrow {
  color: #94a3b8;
  margin-left: 12px;
}

.chat-input-area {
  padding: 16px 20px;
  background: white;
  border-top: 1px solid #e2e8f0;
}

.input-wrapper {
  display: flex;
  align-items: center;
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 24px;
  padding: 4px 4px 4px 16px;
}

.input-wrapper input {
  flex: 1;
  background: transparent;
  border: none;
  outline: none;
  font-size: 14px;
  color: #1e293b;
  font-weight: 500;
}

.input-wrapper input::placeholder {
  color: #94a3b8;
}

.send-btn {
  background: #1c5bf0;
  color: white;
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: opacity 0.2s;
}

.send-btn:disabled {
  opacity: 0.5;
  cursor: default;
}

.end-modal-backdrop {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.5);
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: center;
  animation: fadeIn 0.3s ease;
}

.end-modal-content {
  background: white;
  width: 90%;
  max-width: 340px;
  border-radius: 24px;
  padding: 32px 24px;
  text-align: center;
  box-shadow: 0 10px 40px rgba(0,0,0,0.2);
}

.end-modal-content h2 {
  color: #1a235c;
  font-size: 24px;
  font-weight: 800;
  margin: 0 0 12px 0;
}

.end-modal-content p {
  color: #64748b;
  font-size: 15px;
  line-height: 1.5;
  font-weight: 500;
  margin: 0 0 24px 0;
}

.end-actions {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.primary-btn, .secondary-btn, .outline-btn {
  width: 100%;
  padding: 14px;
  border-radius: 100px;
  font-weight: 700;
  font-size: 14px;
  border: none;
  cursor: pointer;
  transition: transform 0.2s;
}

.primary-btn:active, .secondary-btn:active, .outline-btn:active {
  transform: scale(0.98);
}

.primary-btn {
  background: #1c5bf0;
  color: white;
  box-shadow: 0 4px 12px rgba(28, 91, 240, 0.2);
}

.secondary-btn {
  background: #f0f4ff;
  color: #1c5bf0;
}

.outline-btn {
  background: transparent;
  color: #64748b;
}

</style>
