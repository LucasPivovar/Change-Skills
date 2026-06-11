<template>
  <div class="view-container">
    <!-- Header -->
    <header class="header">
      <button class="back-btn" @click="$emit('goBack')">
        <ChevronLeftIcon size="24" stroke-width="2.5" />
      </button>
      <h2>Meu Perfil</h2>
      <button class="settings-btn">
        <SettingsIcon size="24" />
      </button>
    </header>

    <div class="content-scroll">
      <!-- Profile Header -->
      <div class="profile-header">
        <div class="avatar-wrapper">
          <img src="https://i.pravatar.cc/150?img=11" class="profile-avatar" />
          <button class="edit-avatar-btn">
            <CameraIcon size="16" />
          </button>
        </div>
        <div class="name-container">
          <h1 class="profile-name" v-if="!isEditingName">{{ profileName }}</h1>
          <input 
            v-else 
            type="text" 
            v-model="newName" 
            @keyup.enter="saveName" 
            @blur="saveName" 
            class="name-input" 
            ref="nameInputRef"
          />
          <button class="edit-name-btn" @click="toggleEditName">
            <PencilIcon v-if="!isEditingName" size="16" />
            <CheckIcon v-else size="16" />
          </button>
        </div>
      </div>

      <!-- Stats Grid -->
      <div class="stats-grid">
        <div class="stat-card full-width" style="animation-delay: 0.1s">
          <div class="stat-icon-wrapper fire">
            <span class="fire-badge-emoji">🔥</span>
          </div>
          <div class="stat-info">
            <span class="stat-value">5 dias de ofensiva</span>
            <span class="stat-label">Praticando diariamente!</span>
          </div>
        </div>
        
        <div class="stat-card clickable" style="animation-delay: 0.2s" @click="openFriendsModal">
          <div class="stat-icon-wrapper friends-icon-bg">
            <UsersIcon size="24" class="stat-icon" />
          </div>
          <div class="stat-info">
            <span class="stat-value">7</span>
            <span class="stat-label">Amigos</span>
          </div>
        </div>
        
        <div class="stat-card" style="animation-delay: 0.3s">
          <div class="stat-icon-wrapper time">
            <ClockIcon size="24" class="stat-icon" />
          </div>
          <div class="stat-info">
            <span class="stat-value">4h 30m</span>
            <span class="stat-label">Tempo Praticado</span>
          </div>
        </div>
      </div>

      <!-- Menu Options -->
      <div class="options-list" style="animation-delay: 0.6s">
        <div class="option-item">
          <div class="option-icon-box"><ShieldIcon size="20" /></div>
          <span>Privacidade e Segurança</span>
          <ChevronRightIcon size="20" class="option-arrow" />
        </div>
      </div>
    </div>

    <!-- Friends List Modal -->
    <transition name="fade">
      <div class="modal-backdrop" v-if="isFriendsModalOpen" @click.self="isFriendsModalOpen = false">
        <transition name="slide-up" appear>
          <div class="friends-modal-container">
            <div class="modal-header">
              <h3>Meus Amigos</h3>
              <button @click="isFriendsModalOpen = false" class="close-btn-round"><XIcon size="20" /></button>
            </div>
            
            <div class="friends-list">
              <div class="friend-item" v-for="friend in friends" :key="friend.id">
                <img :src="friend.avatar" class="friend-avatar" />
                <div class="friend-info-row">
                  <span class="friend-name">{{ friend.name }}</span>
                  <span class="friend-streak">🔥 {{ friend.streak }} dias</span>
                </div>
                <button class="view-profile-btn" @click="showFriendProfile(friend)">
                  Ver Perfil
                </button>
              </div>
            </div>
          </div>
        </transition>
      </div>
    </transition>

    <!-- Friend Profile Modal -->
    <transition name="fade">
      <div class="modal-backdrop" v-if="isFriendProfileOpen" @click.self="isFriendProfileOpen = false">
        <transition name="slide-up" appear>
          <div class="profile-modal-container">
            <div class="modal-header-bar">
              <button class="close-btn-round" @click="isFriendProfileOpen = false">
                <XIcon size="20" />
              </button>
            </div>
            
            <div class="profile-modal-content">
              <div class="profile-avatar-container">
                <img :src="selectedFriend?.avatar" class="modal-profile-avatar" />
              </div>
              
              <h2 class="modal-profile-name">{{ selectedFriend?.name }}</h2>
              
              <div class="friend-stats-container">
                <div class="friend-stat-banner streak-banner">
                  <span class="fire-emoji">🔥</span>
                  <div class="banner-info">
                    <strong>{{ selectedFriend?.streak }} dias</strong>
                    <span>Vocês estão em sequência!</span>
                  </div>
                </div>
                
                <div class="friend-stat-banner lang-banner">
                  <span class="lang-emoji">🌍</span>
                  <div class="banner-info">
                    <strong>Praticando {{ selectedFriend?.language }}</strong>
                    <span>Idioma em foco</span>
                  </div>
                </div>
              </div>
              
              <button class="modal-chat-btn" @click="goToChatFromProfile">
                Conversar agora
              </button>
              
              <button class="modal-unfriend-btn" @click="unfriendAction">
                Desfazer Amizade
              </button>
            </div>
          </div>
        </transition>
      </div>
    </transition>

    <!-- Bottom Navigation -->
    <nav class="bottom-nav">
      <div class="nav-item" @click="$emit('navigate', 'home')">
        <HomeIcon size="24" />
        <span>Início</span>
      </div>
      <div class="nav-item" @click="$emit('navigate', 'conversations')">
        <MessageCircleIcon size="24" />
        <span>Chats</span>
      </div>
      <div class="nav-item active" @click="$emit('navigate', 'profile')">
        <UserIcon size="24" />
        <span>Perfil</span>
      </div>
    </nav>
  </div>
</template>

<script setup>
import { ref, nextTick } from 'vue'
import { 
  ChevronLeftIcon, 
  SettingsIcon, 
  CameraIcon, 
  ClockIcon, 
  ChevronRightIcon,
  UserIcon,
  ShieldIcon,
  Pencil as PencilIcon,
  Check as CheckIcon,
  Users as UsersIcon,
  X as XIcon,
  Home as HomeIcon,
  MessageCircle as MessageCircleIcon
} from '@lucide/vue'

const emit = defineEmits(['goBack', 'navigate'])

const isEditingName = ref(false)
const profileName = ref('João Silva')
const newName = ref('João Silva')
const nameInputRef = ref(null)

const toggleEditName = () => {
  if (isEditingName.value) {
    saveName()
  } else {
    isEditingName.value = true
    newName.value = profileName.value
    nextTick(() => {
      if (nameInputRef.value) {
        nameInputRef.value.focus()
      }
    })
  }
}

const saveName = () => {
  if (newName.value.trim() !== '') {
    profileName.value = newName.value
  }
  isEditingName.value = false
}

// Friends List logic
const isFriendsModalOpen = ref(false)
const isFriendProfileOpen = ref(false)
const selectedFriend = ref(null)

const friends = ref([
  { id: 1, name: 'David Smith', avatar: 'https://i.pravatar.cc/150?img=12', streak: 7, language: 'Inglês' },
  { id: 2, name: 'Ana Souza', avatar: 'https://i.pravatar.cc/150?img=47', streak: 3, language: 'Espanhol' },
  { id: 3, name: 'Carlos', avatar: 'https://i.pravatar.cc/150?img=11', streak: 5, language: 'Inglês' },
  { id: 4, name: 'Lucas', avatar: 'https://i.pravatar.cc/150?img=33', streak: 5, language: 'Francês' },
  { id: 5, name: 'Marcos', avatar: 'https://i.pravatar.cc/150?img=8', streak: 12, language: 'Inglês' },
  { id: 6, name: 'Sofia', avatar: 'https://i.pravatar.cc/150?img=9', streak: 8, language: 'Espanhol' },
  { id: 7, name: 'Julia', avatar: 'https://i.pravatar.cc/150?img=5', streak: 4, language: 'Português' }
])

const openFriendsModal = () => {
  isFriendsModalOpen.value = true
}

const showFriendProfile = (friend) => {
  selectedFriend.value = friend
  isFriendProfileOpen.value = true
}

const goToChatFromProfile = () => {
  isFriendProfileOpen.value = false
  isFriendsModalOpen.value = false
  emit('navigate', 'conversations')
}

const unfriendAction = () => {
  if (confirm(`Tem certeza que deseja desfazer a amizade com ${selectedFriend.value.name}?`)) {
    friends.value = friends.value.filter(f => f.id !== selectedFriend.value.id)
    isFriendProfileOpen.value = false
  }
}
</script>

<style scoped>
.view-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #f8fafc;
  z-index: 50;
  display: flex;
  flex-direction: column;
  animation: slideIn 0.3s ease;
}

@keyframes slideIn {
  from { transform: translateX(100%); }
  to { transform: translateX(0); }
}

@keyframes slideUpFade {
  from { opacity: 0; transform: translateY(15px); }
  to { opacity: 1; transform: translateY(0); }
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  background: white;
  border-bottom: 1px solid #f1f5f9;
}

.back-btn, .settings-btn {
  background: none;
  border: none;
  color: #1a235c;
  cursor: pointer;
  padding: 8px;
  margin: -8px;
}

h2 {
  margin: 0;
  font-size: 18px;
  color: #1a235c;
  font-weight: 800;
}

.content-scroll {
  flex: 1;
  overflow-y: auto;
  padding: 24px 20px;
  display: flex;
  flex-direction: column;
  gap: 28px;
  padding-bottom: 100px;
}

.profile-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  opacity: 0;
  animation: slideUpFade 0.4s ease forwards;
}

.avatar-wrapper {
  position: relative;
  margin-bottom: 8px;
}

.profile-avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
  border: 4px solid white;
  box-shadow: 0 4px 16px rgba(0,0,0,0.08);
}

.edit-avatar-btn {
  position: absolute;
  bottom: 0;
  right: 0;
  background: #1c5bf0;
  color: white;
  border: none;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 3px solid white;
  cursor: pointer;
}

.name-container {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-top: 4px;
}

.profile-name {
  margin: 0;
  font-size: 22px;
  font-weight: 800;
  color: #1e293b;
}

.name-input {
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  padding: 4px 8px;
  font-size: 18px;
  font-weight: 700;
  color: #1e293b;
  outline: none;
  width: 160px;
}

.edit-name-btn {
  background: #f1f5f9;
  border: none;
  color: #64748b;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.2s;
}

.edit-name-btn:hover {
  background: #e2e8f0;
  color: #1a235c;
}

.stats-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
}

.stat-card {
  background: white;
  border-radius: 20px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.03);
  opacity: 0;
  animation: slideUpFade 0.4s ease forwards;
}

.stat-card.full-width {
  grid-column: span 2;
  background: linear-gradient(135deg, #fff1f2 0%, #ffe4e6 100%);
  border: 1px solid #fecdd3;
  padding: 20px 24px;
}

.stat-card.clickable {
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
}

.stat-card.clickable:active {
  transform: scale(0.97);
}

.stat-icon-wrapper {
  width: 44px;
  height: 44px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.stat-icon-wrapper.fire { 
  background: white; 
  box-shadow: 0 4px 12px rgba(239, 68, 68, 0.15);
}

.fire-badge-emoji {
  font-size: 24px;
}

.stat-icon-wrapper.friends-icon-bg { 
  background: #e0f2fe; 
  color: #0ea5e9; 
}

.stat-icon-wrapper.time { 
  background: #fef3c7; 
  color: #f59e0b; 
}

.stat-info {
  display: flex;
  flex-direction: column;
}

.stat-value {
  font-size: 16px;
  font-weight: 800;
  color: #1e293b;
}

.stat-label {
  font-size: 12px;
  color: #64748b;
  font-weight: 600;
}

.options-list {
  background: white;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.03);
  display: flex;
  flex-direction: column;
  opacity: 0;
  animation: slideUpFade 0.4s ease forwards;
}

.option-item {
  display: flex;
  align-items: center;
  padding: 16px 20px;
  border-bottom: 1px solid #f1f5f9;
  cursor: pointer;
  transition: background 0.2s;
}

.option-item:last-child {
  border-bottom: none;
}

.option-item:hover {
  background: #f8fafc;
}

.option-icon-box {
  color: #64748b;
  margin-right: 16px;
}

.option-item span {
  flex: 1;
  font-size: 15px;
  font-weight: 700;
  color: #1e293b;
}

.option-arrow {
  color: #cbd5e1;
}

/* Bottom Navigation */
.bottom-nav {
  display: flex;
  justify-content: space-around;
  padding: 12px 20px;
  background: white;
  border-top: 1px solid #e2e8f0;
  padding-bottom: calc(env(safe-area-inset-bottom, 12px) + 24px);
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  z-index: 50;
}

.nav-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  color: #94a3b8;
  cursor: pointer;
  transition: color 0.2s;
}

.nav-item.active {
  color: #1c5bf0;
}

.nav-item span {
  font-size: 11px;
  font-weight: 500;
}

/* Modals & Overlays */
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.4);
  z-index: 200;
  display: flex;
  align-items: flex-end;
  justify-content: center;
}

.friends-modal-container, .profile-modal-container {
  width: 100%;
  max-width: 480px;
  background: white;
  border-radius: 32px 32px 0 0;
  box-shadow: 0 -10px 40px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  max-height: 80%;
  padding-bottom: 24px;
}

.profile-modal-container {
  background: #f8fafc;
  padding-bottom: 32px;
}

.modal-header, .modal-header-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 24px;
}

.modal-header-bar {
  justify-content: flex-end;
  padding: 16px 24px;
}

.modal-header h3 {
  color: #1a235c;
  font-size: 18px;
  font-weight: 800;
  margin: 0;
}

.close-btn-round {
  background: #f1f5f9;
  border: none;
  color: #64748b;
  cursor: pointer;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s;
}

.close-btn-round:hover {
  background: #e2e8f0;
}

.friends-list {
  flex: 1;
  overflow-y: auto;
  padding: 0 24px 24px;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.friend-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding-bottom: 12px;
  border-bottom: 1px solid #f1f5f9;
}

.friend-item:last-child {
  border-bottom: none;
}

.friend-avatar {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  object-fit: cover;
}

.friend-info-row {
  display: flex;
  flex-direction: column;
  flex: 1;
  gap: 2px;
}

.friend-name {
  font-size: 15px;
  font-weight: 800;
  color: #1e293b;
}

.friend-streak {
  font-size: 12px;
  color: #e11d48;
  font-weight: 600;
}

.view-profile-btn {
  background: #f0f4ff;
  color: #1c5bf0;
  border: none;
  padding: 8px 14px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 700;
  cursor: pointer;
  transition: background 0.2s;
}

.view-profile-btn:hover {
  background: #e0e7ff;
}

/* Friend profile detail modal content */
.profile-modal-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 24px;
}

.profile-avatar-container {
  position: relative;
  margin-bottom: 16px;
}

.modal-profile-avatar {
  width: 90px;
  height: 90px;
  border-radius: 50%;
  object-fit: cover;
  border: 4px solid white;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
}

.modal-profile-name {
  font-size: 22px;
  font-weight: 800;
  color: #1a235c;
  margin: 0 0 24px 0;
}

.friend-stats-container {
  display: flex;
  flex-direction: column;
  gap: 16px;
  width: 100%;
  margin-bottom: 32px;
}

.friend-stat-banner {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px 20px;
  border-radius: 20px;
  background: white;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.02);
  border: 1px solid #e2e8f0;
}

.fire-emoji, .lang-emoji {
  font-size: 24px;
}

.banner-info {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.banner-info strong {
  font-size: 16px;
  color: #1e293b;
  font-weight: 800;
}

.banner-info span {
  font-size: 12px;
  color: #64748b;
  font-weight: 500;
}

.modal-chat-btn {
  background: #1c5bf0;
  color: white;
  border: none;
  border-radius: 100px;
  padding: 14px;
  font-weight: 700;
  font-size: 15px;
  cursor: pointer;
  transition: background 0.2s, transform 0.1s;
  width: 100%;
  text-align: center;
  box-shadow: 0 4px 12px rgba(28, 91, 240, 0.2);
}

.modal-chat-btn:active {
  transform: scale(0.98);
}

.modal-unfriend-btn {
  background: transparent;
  color: #ef4444;
  border: 2px dashed #fca5a5;
  border-radius: 100px;
  padding: 12px;
  font-weight: 700;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.2s;
  width: 100%;
  text-align: center;
  margin-top: 12px;
}

.modal-unfriend-btn:hover {
  background: #fff5f5;
  border-color: #ef4444;
}

.friend-stat-banner.streak-banner {
  background: linear-gradient(135deg, #fff1f2 0%, #ffe4e6 100%);
  border: 1px solid #fecdd3;
  box-shadow: 0 4px 12px rgba(239, 68, 68, 0.08);
}

/* Modal transitions */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.slide-up-enter-active,
.slide-up-leave-active {
  transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}
.slide-up-enter-from,
.slide-up-leave-to {
  transform: translateY(100%);
}
</style>
