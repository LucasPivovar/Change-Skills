<template>
  <div>
    <!-- Overlay/Backdrop -->
    <transition name="fade">
      <div 
        class="sidebar-backdrop" 
        v-if="isOpen" 
        @click="$emit('close')"
      ></div>
    </transition>

    <!-- Sidebar Panel -->
    <transition name="slide">
      <div class="sidebar" v-if="isOpen">
        
        <!-- Profile Section -->
        <div class="profile-section">
          <img src="https://i.pravatar.cc/150?img=11" alt="Profile" class="profile-avatar" />
          <div class="profile-info">
            <span class="profile-name">João Silva</span>
            <span class="view-profile" @click="handleNav('profile')">Ver perfil</span>
          </div>
        </div>

        <!-- Navigation Links -->
        <nav class="nav-links">
          <div class="nav-item" @click="handleNav('home')">
            <HomeIcon size="22" class="nav-icon" />
            <span>Início</span>
          </div>
          
          <div class="nav-item" @click="handleNav('conversations')">
            <MessageCircleIcon size="22" class="nav-icon" />
            <span>Conversas</span>
          </div>
          
          <div class="nav-item" @click="handleNav('language')">
            <GlobeIcon size="22" class="nav-icon" />
            <span>Mudar Idioma</span>
          </div>
          
          <div class="nav-item logout" @click="handleNav('login')">
            <LogOutIcon size="22" class="nav-icon" />
            <span>Sair</span>
          </div>
        </nav>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { 
  Home as HomeIcon,
  MessageCircle as MessageCircleIcon,
  Globe as GlobeIcon,
  LogOut as LogOutIcon
} from '@lucide/vue'

defineProps({
  isOpen: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['close', 'navigate'])

const handleNav = (route) => {
  emit('navigate', route)
  emit('close')
}
</script>

<style scoped>
.sidebar-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.4);
  z-index: 100;
}

.sidebar {
  position: fixed;
  top: 0;
  left: 0;
  width: 280px;
  height: 100vh;
  background-color: white;
  z-index: 101;
  box-shadow: 4px 0 24px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  padding: 32px 24px;
}

/* Animations */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.slide-enter-active,
.slide-leave-active {
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}
.slide-enter-from,
.slide-leave-to {
  transform: translateX(-100%);
}

.profile-section {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 40px;
  padding-bottom: 24px;
  border-bottom: 1px solid #f1f5f9;
}

.profile-avatar {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  object-fit: cover;
  border: 2px solid #e0e7ff;
}

.profile-info {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.profile-name {
  font-size: 18px;
  font-weight: 800;
  color: #1a235c;
}

.view-profile {
  font-size: 13px;
  color: #1c5bf0;
  font-weight: 600;
  cursor: pointer;
}

.nav-links {
  display: flex;
  flex-direction: column;
  gap: 8px;
  flex: 1;
}

.nav-item {
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 14px 16px;
  border-radius: 12px;
  color: #475569;
  font-weight: 700;
  font-size: 15px;
  cursor: pointer;
  transition: background-color 0.2s, color 0.2s;
}

.nav-item:hover, .nav-item:active {
  background-color: #f4f8ff;
  color: #1c5bf0;
}

.nav-icon {
  color: #1c5bf0;
}

.nav-item.logout {
  margin-top: auto;
  background-color: #fff1f2;
  color: #e11d48;
}

.nav-item.logout .nav-icon {
  color: #e11d48;
}

.nav-item.logout:hover, .nav-item.logout:active {
  background-color: #ffe4e6;
  color: #be123c;
}
</style>
