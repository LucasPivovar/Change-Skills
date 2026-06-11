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
        <h1 class="profile-name">João Silva</h1>
      </div>

      <!-- Stats Grid -->
      <div class="stats-grid">
        <div class="stat-card" style="animation-delay: 0.1s">
          <div class="stat-icon-wrapper fire">
            <FlameIcon size="24" class="stat-icon" />
          </div>
          <div class="stat-info">
            <span class="stat-value">5 dias</span>
            <span class="stat-label">Ofensiva</span>
          </div>
        </div>
        
        <div class="stat-card" style="animation-delay: 0.2s">
          <div class="stat-icon-wrapper message">
            <MessageCircleIcon size="24" class="stat-icon" />
          </div>
          <div class="stat-info">
            <span class="stat-value">12</span>
            <span class="stat-label">Conversas</span>
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
          <div class="option-icon-box"><UserIcon size="20" /></div>
          <span>Editar Informações</span>
          <ChevronRightIcon size="20" class="option-arrow" />
        </div>
        <div class="option-item">
          <div class="option-icon-box"><BellIcon size="20" /></div>
          <span>Notificações</span>
          <ChevronRightIcon size="20" class="option-arrow" />
        </div>
        <div class="option-item">
          <div class="option-icon-box"><ShieldIcon size="20" /></div>
          <span>Privacidade e Segurança</span>
          <ChevronRightIcon size="20" class="option-arrow" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { 
  ChevronLeftIcon, 
  SettingsIcon, 
  CameraIcon, 
  FlameIcon, 
  MessageCircleIcon, 
  ClockIcon, 
  ChevronRightIcon,
  UserIcon,
  BellIcon,
  ShieldIcon
} from '@lucide/vue'

defineEmits(['goBack'])
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
  gap: 32px;
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

.profile-name {
  margin: 0;
  font-size: 24px;
  font-weight: 800;
  color: #1e293b;
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

.stat-icon-wrapper {
  width: 44px;
  height: 44px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.stat-icon-wrapper.fire { background: #fff1f2; color: #ef4444; }
.stat-icon-wrapper.message { background: #e0f2fe; color: #0ea5e9; }
.stat-icon-wrapper.time { background: #fef3c7; color: #f59e0b; }

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
</style>
