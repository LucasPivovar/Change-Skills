<template>
  <div class="chat-container">
    <!-- Header -->
    <header class="chat-header">
      <button class="back-btn" @click="$emit('goBack')">
        <ChevronLeftIcon size="24" stroke-width="2.5" />
      </button>
      <div class="group-info" @click="showParticipants = true" style="cursor: pointer;">
        <div class="group-icon-wrapper">
          <UsersIcon size="20" class="group-icon" />
        </div>
        <div class="group-text">
          <span class="group-name">Sala Global - Iniciantes</span>
          <span class="group-status">
            <span class="online-dot"></span> 12 online
          </span>
        </div>
        <ChevronRightIcon size="20" style="margin-left: auto; color: #94a3b8;" />
      </div>
    </header>

    <div class="objective-wrapper">
      <!-- Topic Box -->
      <div class="topic-box">
        <div class="topic-content">
          <div class="topic-header">
            <MessageSquareIcon size="18" class="topic-icon" />
            <span>TÓPICO DO DIA</span>
          </div>
          <h3 class="topic-title">Filmes e Séries Favoritos</h3>
          <p class="topic-desc">Compartilhe com o grupo o que você tem assistido ultimamente.</p>
        </div>
        <img src="../assets/dica-hero.png" class="topic-mascot" />
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
          <img v-if="!msg.isMine" :src="msg.avatar" class="msg-avatar" />
          
          <div class="message-group">
            <span v-if="!msg.isMine" class="sender-name">{{ msg.name }}</span>
            <div class="message-bubble" :class="msg.isMine ? 'bubble-mine' : 'bubble-theirs'">
              <p>{{ msg.text }}</p>
              <span class="message-time">
                {{ msg.time }}
                <CheckCheckIcon size="12" class="read-receipt" v-if="msg.isMine" />
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Input Footer -->
    <div class="chat-input-area">
      <div class="input-wrapper">
        <input type="text" placeholder="Escreva para o grupo..." v-model="newMessage" @keyup.enter="sendMessage" />
        <button class="send-btn" @click="sendMessage" :disabled="!newMessage.trim()">
          <SendIcon size="18" stroke-width="2.5" />
        </button>
      </div>
    </div>

    <!-- Participants Modal -->
    <div class="participants-modal-backdrop" v-if="showParticipants" @click.self="showParticipants = false">
      <div class="participants-modal-content">
        <div class="modal-header">
          <h3>Membros do Grupo</h3>
          <button @click="showParticipants = false" class="close-btn"><XIcon size="24" /></button>
        </div>
        <div class="participants-list-modal">
          <div class="participant-item" v-for="user in participantsList" :key="user.id">
            <div class="participant-avatar-wrapper">
              <img :src="user.avatar" class="participant-avatar" />
              <span v-if="user.online" class="participant-online-dot"></span>
            </div>
            <div class="participant-info">
              <span class="participant-name">{{ user.name }}</span>
              <span class="participant-status">{{ user.online ? 'Online' : 'Offline' }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, nextTick } from 'vue'
import { 
  ChevronLeftIcon, 
  ChevronRightIcon,
  UsersIcon, 
  MessageSquareIcon,
  SendIcon, 
  CheckCheckIcon,
  XIcon
} from '@lucide/vue'

const emit = defineEmits(['goBack'])

const showParticipants = ref(false)

const participantsList = ref([
  { id: 1, name: 'Você', avatar: 'https://i.pravatar.cc/150?img=1', online: true },
  { id: 2, name: 'Carlos', avatar: 'https://i.pravatar.cc/150?img=11', online: true },
  { id: 3, name: 'Ana', avatar: 'https://i.pravatar.cc/150?img=47', online: true },
  { id: 4, name: 'David', avatar: 'https://i.pravatar.cc/150?img=12', online: true },
  { id: 5, name: 'Julia', avatar: 'https://i.pravatar.cc/150?img=5', online: true },
  { id: 6, name: 'Marcos', avatar: 'https://i.pravatar.cc/150?img=8', online: false },
  { id: 7, name: 'Sofia', avatar: 'https://i.pravatar.cc/150?img=9', online: false }
])

const messagesListRef = ref(null)

const messages = ref([
  { name: 'Carlos', avatar: 'https://i.pravatar.cc/150?img=11', text: 'Hi everyone! How is it going?', time: '10:30', isMine: false },
  { name: 'Ana', avatar: 'https://i.pravatar.cc/150?img=47', text: 'Hello Carlos! I am doing great.', time: '10:31', isMine: false },
  { name: 'You', avatar: '', text: 'Hey guys! Anyone watched that new sci-fi movie?', time: '10:32', isMine: true },
  { name: 'David', avatar: 'https://i.pravatar.cc/150?img=12', text: 'Yes! The visuals were amazing but the story was a bit confusing.', time: '10:33', isMine: false },
  { name: 'Ana', avatar: 'https://i.pravatar.cc/150?img=47', text: 'I agree. I prefer comedies though.', time: '10:35', isMine: false }
])

const newMessage = ref('')

const scrollToBottom = () => {
  nextTick(() => {
    if (messagesListRef.value) {
      messagesListRef.value.scrollTop = messagesListRef.value.scrollHeight
    }
  })
}

onMounted(() => {
  scrollToBottom()
})

const sendMessage = () => {
  if (newMessage.value.trim() === '') return
  
  messages.value.push({
    name: 'You',
    avatar: '',
    text: newMessage.value,
    time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }),
    isMine: true
  })
  newMessage.value = ''
  scrollToBottom()
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

.group-info {
  display: flex;
  align-items: center;
  flex: 1;
  margin-left: 8px;
}

.group-icon-wrapper {
  width: 42px;
  height: 42px;
  background: #e0e7ff;
  color: #1c5bf0;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 12px;
}

.group-text {
  display: flex;
  flex-direction: column;
}

.group-name {
  font-size: 16px;
  font-weight: 800;
  color: #1a235c;
  margin-bottom: 2px;
}

.group-status {
  font-size: 12px;
  color: #64748b;
  font-weight: 600;
  display: flex;
  align-items: center;
  gap: 6px;
}

.online-dot {
  width: 8px;
  height: 8px;
  background-color: #22c55e;
  border-radius: 50%;
}

.objective-wrapper {
  padding: 20px 20px 0 20px;
  background: #f4f8ff;
  z-index: 10;
  flex-shrink: 0;
}

.topic-box {
  background: linear-gradient(135deg, #ffffff 0%, #f0f4ff 100%);
  border-radius: 20px;
  padding: 20px;
  display: flex;
  align-items: flex-start;
  position: relative;
  box-shadow: 0 8px 24px rgba(28, 91, 240, 0.12);
  border: 1px solid #c7d2fe;
}

.topic-content {
  flex: 1;
  padding-right: 60px; /* Space for mascot */
}

.topic-header {
  display: flex;
  align-items: center;
  gap: 8px;
  color: #1c5bf0;
  font-weight: 800;
  font-size: 13px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 8px;
}

.topic-title {
  color: #1a235c;
  font-size: 18px;
  font-weight: 800;
  margin: 0 0 4px 0;
}

.topic-desc {
  font-size: 13px;
  color: #475569;
  line-height: 1.4;
  font-weight: 600;
  margin: 0;
}

.topic-mascot {
  position: absolute;
  right: -10px;
  bottom: 0;
  width: 90px;
  height: 90px;
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

.msg-avatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  margin-bottom: 8px;
}

.message-group {
  display: flex;
  flex-direction: column;
  max-width: 75%;
}

.sender-name {
  font-size: 11px;
  color: #64748b;
  font-weight: 700;
  margin-bottom: 4px;
  margin-left: 4px;
}

.message-bubble {
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

.participants-modal-backdrop {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.5);
  z-index: 100;
  display: flex;
  align-items: flex-end;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

.participants-modal-content {
  background: white;
  width: 100%;
  max-height: 80%;
  border-radius: 24px 24px 0 0;
  padding: 24px;
  display: flex;
  flex-direction: column;
  animation: slideUp 0.3s ease;
}

@keyframes slideUp {
  from { transform: translateY(100%); }
  to { transform: translateY(0); }
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.modal-header h3 {
  color: #1a235c;
  font-size: 18px;
  font-weight: 800;
  margin: 0;
}

.close-btn {
  background: none;
  border: none;
  color: #64748b;
  cursor: pointer;
  padding: 4px;
}

.participants-list-modal {
  flex: 1;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.participant-item {
  display: flex;
  align-items: center;
  gap: 12px;
}

.participant-avatar-wrapper {
  position: relative;
}

.participant-avatar {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  object-fit: cover;
}

.participant-online-dot {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 12px;
  height: 12px;
  background-color: #22c55e;
  border: 2px solid white;
  border-radius: 50%;
}

.participant-info {
  display: flex;
  flex-direction: column;
}

.participant-name {
  font-size: 15px;
  font-weight: 700;
  color: #1e293b;
}

.participant-status {
  font-size: 12px;
  color: #94a3b8;
  font-weight: 500;
}
</style>
