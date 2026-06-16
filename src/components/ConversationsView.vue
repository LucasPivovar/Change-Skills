<template>
  <div class="view-container">
    <header class="app-header">
      <div class="header-logo-container" @click="$emit('navigate', 'home')" style="cursor: pointer;">
        <img src="../assets/img/logo-change.png" class="header-logo" alt="Change" />
      </div>
      <h2 class="header-title">{{ t('nav_chats') }}</h2>
      <button class="menu-btn" @click="$emit('openSidebar')">
        <MenuIcon size="24" stroke-width="2.5" />
      </button>
    </header>

    <div class="content-scroll">
      <!-- Mascot suggestion -->
      <div class="mascot-banner">
        <img src="../assets/2.png" class="banner-mascot" />
        <div class="banner-text">
          <strong>{{ t('keep_practicing') }}</strong>
          <p>{{ t('keep_practicing_desc') }}</p>
        </div>
      </div>

      <!-- Últimas Conversas (Chat list) -->
      <div class="chat-list">
        <div class="chat-item" v-for="(chat, index) in activeChats" :key="chat.id" @click="$emit('openChat')" :style="{ animationDelay: (index * 0.1) + 's' }">
          <div class="avatar-wrapper">
            <img :src="chat.avatar" class="chat-avatar" />
            <span class="online-indicator" v-if="chat.online"></span>
          </div>
          <div class="chat-info">
            <div class="chat-header-row">
              <span class="chat-name">{{ chat.name }}</span>
              <span class="chat-time">{{ chat.time }}</span>
            </div>
            <div class="chat-preview-row">
              <span class="chat-preview">{{ chat.lastMessage }}</span>
              <div class="streak-badge" v-if="chat.streak > 0">
                <span style="display: inline-flex; align-items: center; gap: 4px;"><StreakFlame size="16" /> {{ chat.streak }} {{ t('streak_days') }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Convites (Pending Requests) -->
      <div class="section-container" v-if="pendingRequests.length > 0">
        <h3 class="section-title">{{ t('pending_requests') }}</h3>
        <div class="request-list">
          <div class="request-item" v-for="(req, index) in pendingRequests" :key="req.id" :style="{ animationDelay: (index * 0.1) + 's' }">
            <img :src="req.avatar" class="req-avatar" />
            <div class="req-info">
              <span class="req-name">{{ req.name }}</span>
              <span class="req-desc">{{ t('wants_to_practice') }}</span>
            </div>
            <div class="req-actions">
              <button class="accept-btn"><CheckIcon size="18" /></button>
              <button class="reject-btn"><XIcon size="18" /></button>
            </div>
          </div>
        </div>
      </div>

      <!-- Amigos (Enriched Friends List) -->
      <div class="section-container">
        <h3 class="section-title">{{ t('friends_label') }}</h3>
        <div class="friends-list-container">
          <div 
            class="friend-list-item" 
            v-for="(friend, index) in friends" 
            :key="friend.id" 
            @click="showFriendProfile(friend)"
            :style="{ animationDelay: (index * 0.05) + 's' }"
          >
            <div class="avatar-wrapper">
              <img :src="friend.avatar" class="friend-list-avatar" />
              <span class="online-indicator" v-if="friend.online"></span>
            </div>
            <div class="friend-list-info">
              <div class="friend-name-row">
                <span class="friend-name">{{ friend.name }}</span>
                <span class="friend-streak" v-if="friend.streak > 0" style="display: inline-flex; align-items: center; gap: 4px;"><StreakFlame size="14" /> {{ friend.streak }} {{ t('streak_days') }}</span>
              </div>
              <div class="friend-meta-row" style="display: flex; align-items: center; gap: 8px;">
                <span class="friend-lang" style="display: inline-flex; align-items: center; gap: 4px;">
                  <CountryFlagMap :language="friend.language" size="14" />
                  {{ friend.language }}
                </span>
                <span class="friend-level">• {{ friend.level }}</span>
              </div>
            </div>
            <ChevronRightIcon size="20" class="arrow-icon" />
          </div>
        </div>
      </div>
    </div>

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
                  <span class="fire-emoji" style="display: inline-flex;"><StreakFlame size="28" /></span>
                  <div class="banner-info">
                    <strong>{{ selectedFriend?.streak }} {{ t('streak_days') }}</strong>
                    <span>Vocês estão em sequência!</span>
                  </div>
                </div>
                
                <div class="friend-stat-banner lang-banner">
                  <span class="lang-emoji" style="display: inline-flex;">
                    <CountryFlagMap :language="selectedFriend?.language" size="32" />
                  </span>
                  <div class="banner-info">
                    <strong>Praticando {{ selectedFriend?.language }}</strong>
                    <span>Idioma em foco ({{ selectedFriend?.level }})</span>
                  </div>
                </div>
              </div>
              
              <button class="modal-chat-btn" @click="startChatWithFriend">
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
        <HomeIcon size="28" />
        <span>{{ t('nav_home') }}</span>
      </div>
      <div class="nav-item active" @click="$emit('navigate', 'conversations')">
        <MessageCircleIcon size="28" />
        <span>{{ t('nav_chats') }}</span>
      </div>
      <div class="nav-item" @click="$emit('navigate', 'profile')">
        <UserIcon size="28" />
        <span>{{ t('nav_profile') }}</span>
      </div>
    </nav>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { 
  Menu as MenuIcon, 
  Check as CheckIcon, 
  X as XIcon, 
  Home as HomeIcon, 
  MessageCircle as MessageCircleIcon, 
  User as UserIcon,
  ChevronRight as ChevronRightIcon
} from '@lucide/vue'
import { t } from '../data/translations.js'
import StreakFlame from './StreakFlame.vue'
import CountryFlagMap from './CountryFlagMap.vue'

const emit = defineEmits(['openSidebar', 'openChat', 'navigate'])

const activeChats = ref([
  { id: 1, name: 'David Smith', avatar: 'https://i.pravatar.cc/150?img=12', online: true, lastMessage: 'That sounds great! See you.', time: '10:45', streak: 7 },
  { id: 2, name: 'Ana Souza', avatar: 'https://i.pravatar.cc/150?img=47', online: false, lastMessage: 'I really love comedies.', time: 'Ontem', streak: 3 },
  { id: 3, name: 'Carlos Santos', avatar: 'https://i.pravatar.cc/150?img=11', online: true, lastMessage: 'Haha yes, exactly!', time: 'Ontem', streak: 0 }
])

const pendingRequests = ref([
  { id: 1, name: 'Lucas Lima', avatar: 'https://i.pravatar.cc/150?img=33', streak: 5 }
])

const friends = ref([
  { id: 1, name: 'David Smith', avatar: 'https://i.pravatar.cc/150?img=12', streak: 7, language: 'Inglês', level: 'Intermediário', online: true },
  { id: 2, name: 'Ana Souza', avatar: 'https://i.pravatar.cc/150?img=47', streak: 3, language: 'Espanhol', level: 'Avançado', online: false },
  { id: 3, name: 'Carlos Santos', avatar: 'https://i.pravatar.cc/150?img=11', streak: 5, language: 'Inglês', level: 'Iniciante', online: true },
  { id: 4, name: 'Lucas Lima', avatar: 'https://i.pravatar.cc/150?img=33', streak: 5, language: 'Francês', level: 'Intermediário', online: false },
  { id: 5, name: 'Marcos Oliveira', avatar: 'https://i.pravatar.cc/150?img=8', streak: 12, language: 'Inglês', level: 'Avançado', online: true },
  { id: 6, name: 'Sofia Ramos', avatar: 'https://i.pravatar.cc/150?img=9', streak: 8, language: 'Espanhol', level: 'Iniciante', online: false },
  { id: 7, name: 'Julia Costa', avatar: 'https://i.pravatar.cc/150?img=5', streak: 4, language: 'Português', level: 'Intermediário', online: true }
])

const selectedFriend = ref(null)
const isFriendProfileOpen = ref(false)

const showFriendProfile = (person) => {
  selectedFriend.value = {
    name: person.name,
    avatar: person.avatar,
    streak: person.streak || 0,
    language: person.language || 'Inglês',
    level: person.level || 'Intermediário'
  }
  isFriendProfileOpen.value = true
}

const startChatWithFriend = () => {
  isFriendProfileOpen.value = false
  emit('openChat')
}

const unfriendAction = () => {
  if (confirm(t('unfriend_confirm'))) {
    activeChats.value = activeChats.value.filter(c => c.name !== selectedFriend.value.name)
    friends.value = friends.value.filter(f => f.name !== selectedFriend.value.name)
    pendingRequests.value = pendingRequests.value.filter(r => r.name !== selectedFriend.value.name)
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
  background: white;
  z-index: 10;
  display: flex;
  flex-direction: column;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUpFade {
  from { opacity: 0; transform: translateY(15px); }
  to { opacity: 1; transform: translateY(0); }
}

.app-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  background: #ffffff;
  border-bottom: 1px solid #e2e8f0;
  width: 100%;
  height: 64px;
  position: relative;
  z-index: 100;
  flex-shrink: 0;
}

.header-logo-container {
  display: flex;
  align-items: center;
}

.header-logo {
  height: 42px;
  object-fit: contain;
}

.header-title {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  margin: 0;
  font-size: 18px;
  color: #1a235c;
  font-weight: 800;
  white-space: nowrap;
}

.menu-btn {
  background: none;
  border: none;
  color: #1a235c;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 4px;
  transition: transform 0.2s;
}

.menu-btn:active {
  transform: scale(0.95);
}

.content-scroll {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 28px;
  padding-bottom: 140px; /* Increased bottom padding to prevent bottom nav cut-off */
}

.mascot-banner {
  background: #f0f4ff;
  border-radius: 16px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
  border: 1px solid #dbeafe;
}

.banner-mascot {
  width: 56px;
  height: 56px;
  object-fit: contain;
  mix-blend-mode: multiply;
}

.banner-text {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.banner-text strong {
  color: #1c5bf0;
  font-size: 15px;
}

.banner-text p {
  margin: 0;
  font-size: 13px;
  color: #64748b;
  line-height: 1.4;
}

.chat-list {
  display: flex;
  flex-direction: column;
}

.chat-item {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 16px 0;
  border-bottom: 1px solid #f1f5f9;
  cursor: pointer;
  opacity: 0;
  animation: slideUpFade 0.4s ease forwards;
}

.chat-item:last-child {
  border-bottom: none;
}

.avatar-wrapper {
  position: relative;
  flex-shrink: 0;
}

.chat-avatar {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  object-fit: cover;
}

.online-indicator {
  position: absolute;
  bottom: 2px;
  right: 2px;
  width: 14px;
  height: 14px;
  background-color: #22c55e;
  border: 2.5px solid white;
  border-radius: 50%;
}

.chat-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 4px;
  overflow: hidden;
}

.chat-header-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.chat-name {
  font-weight: 800;
  color: #1e293b;
  font-size: 16px;
}

.chat-time {
  font-size: 12px;
  color: #94a3b8;
  font-weight: 600;
}

.chat-preview-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.chat-preview {
  font-size: 14px;
  color: #64748b;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 70%;
}

.streak-badge {
  background: #fff1f2;
  color: #e11d48;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 11px;
  font-weight: 800;
  display: flex;
  align-items: center;
  gap: 4px;
}

.section-container {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.section-title {
  margin: 0;
  font-size: 16px;
  color: #1e293b;
  font-weight: 800;
}

.request-list {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.request-item {
  display: flex;
  align-items: center;
  background: #fff;
  border: 1px solid #e2e8f0;
  border-radius: 16px;
  padding: 16px;
  gap: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.02);
  opacity: 0;
  animation: slideUpFade 0.4s ease forwards;
}

.req-avatar {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  object-fit: cover;
}

.req-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.req-name {
  font-weight: 800;
  color: #1e293b;
  font-size: 15px;
}

.req-desc {
  font-size: 12px;
  color: #64748b;
}

.req-actions {
  display: flex;
  gap: 8px;
}

.accept-btn {
  background: #1c5bf0;
  color: white;
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.reject-btn {
  background: #f1f5f9;
  color: #64748b;
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

/* Enriched Friends List Styles */
.friends-list-container {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.friend-list-item {
  display: flex;
  align-items: center;
  gap: 16px;
  background: white;
  border: 1px solid #e2e8f0;
  border-radius: 20px;
  padding: 16px;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(0,0,0,0.02);
  opacity: 0;
  animation: slideUpFade 0.4s ease forwards;
  transition: transform 0.2s, box-shadow 0.2s;
}

.friend-list-item:active {
  transform: scale(0.98);
}

.friend-list-avatar {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  object-fit: cover;
}

.friend-list-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.friend-name-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.friend-name {
  font-weight: 800;
  color: #1e293b;
  font-size: 16px;
}

.friend-streak {
  font-size: 12px;
  color: #e11d48;
  font-weight: 800;
}

.friend-meta-row {
  display: flex;
  gap: 8px;
  font-size: 13px;
  color: #64748b;
  font-weight: 600;
}

.friend-lang {
  color: #1c5bf0;
}

.friend-level {
  color: #64748b;
}

.arrow-icon {
  color: #cbd5e1;
}

/* Bottom Navigation */
.bottom-nav {
  display: flex;
  justify-content: space-around;
  align-items: center; /* Center items vertically */
  height: 72px; /* Fixed height for vertical centering */
  background: white;
  border-top: 1px solid #e2e8f0;
  padding-bottom: env(safe-area-inset-bottom, 0px);
  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  max-width: 480px;
  z-index: 50;
}

.nav-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2px;
  color: #94a3b8;
  cursor: pointer;
  transition: color 0.2s;
  height: 100%;
}

.nav-item.active {
  color: #1c5bf0;
}

.nav-item span {
  font-size: 11px;
  font-weight: 500;
}

/* Friend Profile Modal Styles */
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

.profile-modal-container {
  width: 100%;
  max-width: 480px;
  background: #f8fafc;
  border-radius: 32px 32px 0 0;
  box-shadow: 0 -10px 40px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  padding-bottom: 32px;
}

.modal-header-bar {
  display: flex;
  justify-content: flex-end;
  padding: 16px 24px;
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

.slide-up-enter-active,
.slide-up-leave-active {
  transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}
.slide-up-enter-from,
.slide-up-leave-to {
  transform: translateY(100%);
}
</style>
