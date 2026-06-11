<template>
  <div class="modal-backdrop" @click.self="$emit('goBack')">
    <transition name="slide-up" appear>
      <div class="mode-container">
    <div class="header-bar">
      <button class="close-btn" @click="$emit('goBack')">
        <XIcon size="28" stroke-width="2.5" />
      </button>
    </div>

    <div class="mode-content">
      <div class="mascot-header-row">
        <div class="text-col">
          <h2>Como você quer<br>praticar hoje?</h2>
        </div>
        <div class="img-col">
          <img src="../assets/header-hero.jpg" class="peeking-mascot" />
        </div>
      </div>
      
      <div class="options-list">


        <div class="option-card solo-card" @click="selectMode('solo')">
          <div class="icon-wrapper solo-icon-wrapper">
            <UsersIcon size="48" class="main-icon" />
            <MessageCircleIcon size="24" class="badge-icon badge-solo" />
          </div>
          <div class="option-text">
            <h3>Conversa com 1 pessoa</h3>
            <p>Encontre um parceiro do seu nível</p>
          </div>
          <ChevronRightIcon class="arrow-icon" size="24" stroke-width="2.5" />
        </div>

        <div class="option-card mascot-card" @click="selectMode('mascot')">
          <div class="icon-wrapper mascot-icon-wrapper">
            <BotIcon size="40" class="main-icon" />
          </div>
          <div class="option-text">
            <h3>Conversa com IA</h3>
            <p>Pratique conversação com nossa inteligência artificial</p>
          </div>
          <ChevronRightIcon class="arrow-icon" size="24" stroke-width="2.5" />
        </div>
      </div>
      <div class="bottom-banner-wrapper">
        <div class="bottom-banner-pill">
          <img src="../assets/dica-hero.png" class="banner-mascot" />
          <span class="banner-text">Escolha e vamos<br>praticar juntos!</span>
        </div>
        </div>
      </div>
    </div>
    </transition>
  </div>
</template>

<script setup>
import { X as XIcon, Users as UsersIcon, MessageCircle as MessageCircleIcon, ChevronRight as ChevronRightIcon, Bot as BotIcon } from '@lucide/vue'

const emit = defineEmits(['goBack', 'selectMode'])

const selectMode = (mode) => {
  emit('selectMode', mode)
}
</script>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.4);
  z-index: 50;
  display: flex;
  align-items: flex-end;
  justify-content: center;
}

.mode-container {
  width: 100%;
  max-width: 480px;
  height: 90%;
  background: #ffffff;
  display: flex;
  flex-direction: column;
  border-radius: 32px 32px 0 0;
  box-shadow: 0 -10px 40px rgba(0,0,0,0.1);
  position: relative;
}

.slide-up-enter-active,
.slide-up-leave-active {
  transition: transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}
.slide-up-enter-from,
.slide-up-leave-to {
  transform: translateY(100%);
}

.header-bar {
  display: flex;
  justify-content: flex-end;
  padding: 16px 24px;
}

.close-btn {
  background: none;
  border: none;
  color: #1a235c;
  cursor: pointer;
  padding: 8px;
}

.mode-content {
  flex: 1;
  padding: 0 24px 24px; /* Removed 120px bottom padding, added standard padding */
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

.mascot-header-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 32px;
}

.text-col h2 {
  font-size: 26px;
  color: #1a235c;
  font-weight: 800;
  line-height: 1.3;
  margin: 0;
}

.img-col {
  display: flex;
  align-items: center;
  justify-content: center;
}

.peeking-mascot {
  height: 85px; /* Made slightly smaller */
  object-fit: contain;
}

.options-list {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.option-card {
  display: flex;
  align-items: center;
  padding: 28px 20px;
  border-radius: 20px;
  border: 2px solid;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
  background: white;
}

.option-card:active {
  transform: scale(0.98);
}

.solo-card {
  border-color: #22c55e;
  box-shadow: 0 8px 24px rgba(34, 197, 94, 0.15);
}

.mascot-card {
  border-color: #0ea5e9;
  box-shadow: 0 8px 24px rgba(14, 165, 233, 0.15);
}

.icon-wrapper {
  width: 72px;
  height: 72px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 20px;
  position: relative;
  flex-shrink: 0;
}

.solo-icon-wrapper {
  background: #dcfce7;
  color: #16a34a;
}

.mascot-icon-wrapper {
  background: #e0f2fe;
  color: #0284c7;
}

.main-icon {
  margin-top: 8px;
}

.badge-icon {
  position: absolute;
  top: -8px;
  right: -4px;
  background: white;
  border-radius: 50%;
  padding: 2px;
}

.badge-solo {
  color: #16a34a;
}

.option-text {
  flex: 1;
}

.option-text h3 {
  font-size: 18px;
  color: #1a235c;
  margin: 0 0 6px 0;
  font-weight: 800;
}

.option-text p {
  font-size: 14px;
  color: #64748b;
  margin: 0;
  line-height: 1.4;
  font-weight: 500;
}

.arrow-icon {
  color: #cbd5e1;
}

.bottom-banner-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 80px; /* Replaces padding-top to give distance from cards without auto margin issues */
  margin-bottom: 24px;
  width: 100%;
}

.bottom-banner-pill {
  position: relative;
  background: #f0f4ff;
  padding: 16px 32px 16px 90px; /* Increased left padding to push text away from mascot */
  border-radius: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 12px rgba(28, 91, 240, 0.05);
  margin-left: 40px; /* Shift the whole block more to the right */
}

.banner-mascot {
  position: absolute;
  left: -70px; /* Pulled slightly more to the left to compensate for size */
  bottom: -24px; /* Glued to the absolute bottom of the container */
  width: 170px; /* Even bigger */
  height: 170px;
  object-fit: contain;
  object-position: bottom left;
  z-index: 5;
}

.banner-text {
  font-size: 18px;
  color: #1a235c;
  font-weight: 800;
  line-height: 1.4;
  text-align: center;
}

@media (max-width: 768px) {
  .bottom-banner-wrapper {
    display: none;
  }
}
</style>
