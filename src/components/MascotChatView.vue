<template>
  <div class="chat-container">
    <!-- Header -->
    <header class="chat-header">
      <button class="back-btn" @click="$emit('goBack')">
        <ChevronLeftIcon size="24" stroke-width="2.5" />
      </button>
      <div class="user-info">
        <div class="mascot-avatar-wrapper">
          <img src="../assets/header-hero.jpg" alt="Camaleão IA" class="avatar" />
        </div>
        <span class="user-name">Camaleão IA</span>
      </div>
      <div class="timer-badge" :class="{ 'warning': timeRemaining <= 60 }">
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
            <span class="mission-title"><strong>Missão:</strong> Planeje uma viagem para Londres</span>
          </div>
          <ChevronDownIcon size="20" class="toggle-icon" :class="{ 'rotated': isMissionOpen }" />
        </div>
        
        <div class="mission-content" v-show="isMissionOpen">
          <p class="mission-desc">Converse com o Camaleão e complete todos os objetivos para finalizar sua missão.</p>
          
          <div class="mission-icons">
            <div class="mission-item" :class="{ 'completed': objectives.hotel }">
              <div class="icon-circle"><BuildingIcon size="20" /></div>
              <span>Reservar hotel</span>
            </div>
            <div class="mission-item" :class="{ 'completed': objectives.food }">
              <div class="icon-circle"><UtensilsIcon size="20" /></div>
              <span>Escolher restaurante</span>
            </div>
            <div class="mission-item" :class="{ 'completed': objectives.directions }">
              <div class="icon-circle"><MapPinIcon size="20" /></div>
              <span>Perguntar direções</span>
            </div>
            <div class="mission-item" :class="{ 'completed': objectives.route }">
              <div class="icon-circle"><RouteIcon size="20" /></div>
              <span>Definir roteiro</span>
            </div>
          </div>
        </div>
        
        <img src="../assets/header-hero.jpg" class="mission-mascot" />
      </div>
    </div>

    <!-- Content -->
    <div class="chat-content" ref="messagesListRef">
      <!-- Messages Area -->
      <div class="messages-list">
        <div 
          v-for="(msg, index) in messages" 
          :key="index"
          :class="['message-row', msg.isMine ? 'mine' : 'theirs']"
        >
          <div v-if="!msg.isMine" class="bot-avatar-wrapper">
            <img src="../assets/header-hero.jpg" class="msg-avatar" />
          </div>
          
          <div class="message-bubble" :class="msg.isMine ? 'bubble-mine' : 'bubble-bot'">
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
        <h2>Missão Concluída!</h2>
        <p>Você completou todos os objetivos ou o tempo acabou. O que quer fazer agora?</p>
        
        <div class="end-actions">
          <button class="primary-btn" @click="$emit('newPartner')">Conversar com alunos</button>
          <button class="secondary-btn" @click="restartMission">Mais uma conversa com a IA</button>
          <button class="outline-btn" @click="$emit('goBack')">Voltar para cursos</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, nextTick } from 'vue'
import { 
  ChevronLeftIcon,
  ChevronDownIcon,
  ClockIcon, 
  FlagIcon, 
  BuildingIcon, 
  UtensilsIcon, 
  MapPinIcon, 
  RouteIcon,
  SendIcon, 
  CheckCheckIcon
} from '@lucide/vue'

const isMissionOpen = ref(false)
const toggleMission = () => {
  isMissionOpen.value = !isMissionOpen.value
}
const emit = defineEmits(['goBack', 'newPartner'])

const timeRemaining = ref(5 * 60)
const showEndModal = ref(false)
let timerInterval = null
const messagesListRef = ref(null)

const formattedTime = computed(() => {
  const minutes = Math.floor(timeRemaining.value / 60)
  const seconds = timeRemaining.value % 60
  return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`
})

const startTimer = () => {
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
  hotel: true,
  food: false,
  directions: false,
  route: false
})

const messages = ref([
  { text: 'Hello! I\'m your travel guide. You will visit London tomorrow. Let\'s prepare your trip together!', time: '10:30', isMine: false },
  { text: 'Hi! I\'m excited. Can you help me find a good hotel?', time: '10:31', isMine: true },
  { text: 'Of course! What type of hotel are you looking for? Budget, comfort, location?', time: '10:31', isMine: false },
  { text: 'I want something comfortable and close to the center.', time: '10:32', isMine: true },
  { text: 'Great choice! I recommend staying near Covent Garden. It\'s central and has beautiful places around.', time: '10:32', isMine: false }
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
}

const restartMission = () => {
  timeRemaining.value = 5 * 60
  showEndModal.value = false
  messages.value = [
    { text: 'Hello again! Where should we travel to this time?', time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }), isMine: false }
  ]
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

.mascot-avatar-wrapper {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: #e0f2fe;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 12px;
  overflow: hidden;
}

.avatar {
  width: 100%;
  height: 100%;
  object-fit: cover;
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
  right: -5px;
  top: -15px;
  width: 80px;
  height: 80px;
  object-fit: contain;
  pointer-events: none;
}

.chat-content {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
  display: flex;
  flex-direction: column;
}

.messages-list {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.message-row {
  display: flex;
  align-items: flex-end;
  gap: 8px;
}

.message-row.mine {
  justify-content: flex-end;
}

.message-row.theirs {
  justify-content: flex-start;
}

.bot-avatar-wrapper {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: #e0f2fe;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 8px;
  overflow: hidden;
}

.msg-avatar {
  width: 100%;
  height: 100%;
  object-fit: cover;
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

.bubble-bot {
  background: #eafbea;
  color: #166534;
  border-radius: 20px 20px 20px 4px;
  border: 1px solid #dcfce7;
}

.bubble-mine {
  background: #e0e7ff;
  color: #1c5bf0;
  border-radius: 20px 20px 4px 20px;
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
  color: #16a34a;
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
