<template>
  <div class="home-container">
    <div class="home-content">
      <button class="menu-btn" @click="$emit('openSidebar')">
        <MenuIcon size="24" stroke-width="2.5" />
      </button>
      <div class="mascot-header">
        <img src="../assets/camaleao.png" class="peeking-mascot" />
        <h2>{{ t('home_title') }}</h2>
        <p>{{ t('home_subtitle') }}</p>
      </div>
      
      <div class="cards-grid">
        <div class="lang-card" @click="$emit('selectLanguage', 'en')" style="animation-delay: 0.1s">
          <img src="../assets/englishcard.jpeg" alt="Inglês" />
          <div class="flag-circle"><img src="https://flagcdn.com/w40/gb.png" alt="UK Flag" /></div>
          <div class="card-label" style="background-color: #21448b;">Inglês</div>
        </div>
        <div class="lang-card" @click="$emit('selectLanguage', 'es')" style="animation-delay: 0.2s">
          <img src="../assets/espanolcard.jpeg" alt="Espanhol" />
          <div class="flag-circle"><img src="https://flagcdn.com/w40/es.png" alt="Spain Flag" /></div>
          <div class="card-label" style="background-color: #b05716;">Espanhol</div>
        </div>
        <div class="lang-card" @click="$emit('selectLanguage', 'fr')" style="animation-delay: 0.3s">
          <img src="../assets/francescard.jpeg" alt="Francês" />
          <div class="flag-circle"><img src="https://flagcdn.com/w40/fr.png" alt="France Flag" /></div>
          <div class="card-label" style="background-color: #28446b;">Francês</div>
        </div>
        <div class="lang-card" @click="$emit('selectLanguage', 'pt')" style="animation-delay: 0.4s">
          <img src="../assets/portuguesecard.jpeg" alt="Português" />
          <div class="flag-circle"><img src="https://flagcdn.com/w40/br.png" alt="Brazil Flag" /></div>
          <div class="card-label" style="background-color: #0b5e28;">Português</div>
        </div>
      </div>
    </div>
    <nav class="bottom-nav">
      <div class="nav-item active" @click="$emit('navigate', 'home')">
        <HomeIcon size="24" />
        <span>{{ t('nav_home') }}</span>
      </div>
      <div class="nav-item" @click="$emit('navigate', 'conversations')">
        <MessageCircleIcon size="24" />
        <span>{{ t('nav_chats') }}</span>
      </div>
      <div class="nav-item" @click="$emit('navigate', 'profile')">
        <UserIcon size="24" />
        <span>{{ t('nav_profile') }}</span>
      </div>
    </nav>
  </div>
</template>

<script setup>
import { 
  Home as HomeIcon, 
  MessageCircle as MessageCircleIcon, 
  User as UserIcon,
  Menu as MenuIcon
} from '@lucide/vue'
import { t } from '../data/translations.js'

defineEmits(['selectLanguage', 'openSidebar', 'navigate'])
</script>

<style scoped>
.home-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #ffffff; /* Changed to pure white */
  z-index: 10; /* Cover the auth background */
  display: flex;
  flex-direction: column;
  animation: fadeIn 0.4s ease;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUpFade {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.menu-btn {
  position: absolute;
  top: 20px;
  left: 20px;
  background: white;
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #1a235c;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  z-index: 100;
  transition: transform 0.2s;
}

.menu-btn:active {
  transform: scale(0.95);
}

.home-content {
  flex: 1;
  padding: 24px;
  overflow-y: auto;
  padding-bottom: 100px; /* space for bottom nav */
  display: flex;
  flex-direction: column;
  align-items: center;
}

.mascot-header {
  text-align: center;
  margin-top: 20px;
  margin-bottom: 32px;
}

.peeking-mascot {
  height: 80px;
  object-fit: contain;
  margin-bottom: -4px;
  margin-top: -20px;
}

.mascot-header h2 {
  font-size: 24px;
  color: #1a235c; /* Dark blue */
  font-weight: 800;
  margin-bottom: 8px;
  line-height: 1.3;
}

.mascot-header p {
  color: #5c6b8a;
  font-size: 15px;
  font-weight: 500;
}

.cards-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
  width: 100%;
  max-width: 400px;
  margin-bottom: 32px;
}

.lang-card {
  position: relative;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
  height: 240px;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
  opacity: 0;
  animation: slideUpFade 0.4s cubic-bezier(0.4, 0, 0.2, 1) forwards;
}

.lang-card:active {
  transform: scale(0.96);
}

.lang-card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.flag-circle {
  position: absolute;
  top: 12px;
  left: 12px;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  box-shadow: 0 2px 6px rgba(0,0,0,0.2);
  z-index: 2;
  overflow: hidden;
  background: white; /* fallback */
  border: 2px solid white;
}

.flag-circle img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-label {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  padding: 12px;
  color: white;
  font-weight: 800;
  font-size: 16px;
  text-align: center;
}

.bottom-nav {
  display: flex;
  justify-content: space-around;
  padding: 12px 20px;
  background: white;
  border-top: 1px solid #e2e8f0;
  padding-bottom: calc(env(safe-area-inset-bottom, 12px) + 24px); /* Extra padding for clearance */
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

.bottom-banner {
  background: rgba(28, 91, 240, 0.08); /* Light blue with opacity */
  border-radius: 24px;
  padding: 12px 24px;
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 20px;
}

.tiny-mascot {
  width: 60px;
  height: 60px;
  object-fit: contain;
  border-radius: 50%; /* Just in case it's a square image */
}

.bottom-banner span {
  font-size: 14px;
  color: #1a235c;
  font-weight: 600;
  line-height: 1.4;
}

@media (max-width: 768px) {
  .home-content {
    padding-bottom: 24px;
  }
}
</style>
