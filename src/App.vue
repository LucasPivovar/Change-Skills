<script setup>
import { ref } from 'vue'
import LanguageSelector from './components/LanguageSelector.vue'
import HeroSection from './components/HeroSection.vue'
import LoginForm from './components/LoginForm.vue'
import RegisterForm from './components/RegisterForm.vue'
import ForgotPasswordForm from './components/ForgotPasswordForm.vue'
import HomeView from './components/HomeView.vue'
import CourseModeView from './components/CourseModeView.vue'
import LanguageCoursesView from './components/LanguageCoursesView.vue'
import ChatView from './components/ChatView.vue'
import GroupChatView from './components/GroupChatView.vue'
import MascotChatView from './components/MascotChatView.vue'
import SidebarMenu from './components/SidebarMenu.vue'
import ConversationsView from './components/ConversationsView.vue'
import ProfileView from './components/ProfileView.vue'

const currentForm = ref('login')
const selectedLanguage = ref(null)
const isSidebarOpen = ref(false)

const handleLanguageSelect = (lang) => {
  selectedLanguage.value = lang
  currentForm.value = 'language-courses'
}

const handleCourseSelect = (course) => {
  currentForm.value = 'course-mode'
}

const handleModeSelect = (mode) => {
  if (mode === 'solo') {
    currentForm.value = 'chat'
  } else if (mode === 'group') {
    currentForm.value = 'group-chat'
  } else if (mode === 'mascot') {
    currentForm.value = 'mascot-chat'
  }
}

const handleNavigation = (route) => {
  if (route === 'login') {
    currentForm.value = 'login'
    selectedLanguage.value = null
  } else if (route === 'language') {
    // Show a basic alert or placeholder since full language switching requires i18n
    alert('Seletor de idioma em breve!')
  } else {
    currentForm.value = route
  }
}
</script>

<template>
  <SidebarMenu 
    :isOpen="isSidebarOpen" 
    @close="isSidebarOpen = false" 
    @navigate="handleNavigation"
  />

  <div class="auth-layout" v-if="currentForm !== 'home' && currentForm !== 'language-courses' && currentForm !== 'course-mode' && currentForm !== 'conversations' && currentForm !== 'profile'">
    <LanguageSelector />
    <HeroSection />
    
    <transition name="slide-fade" mode="out-in">
      <LoginForm 
        v-if="currentForm === 'login'" 
        @goToRegister="currentForm = 'register'" 
        @goToForgot="currentForm = 'forgot'" 
        @loginSuccess="currentForm = 'home'"
      />
      <RegisterForm 
        v-else-if="currentForm === 'register'" 
        @goToLogin="currentForm = 'login'" 
      />
      <ForgotPasswordForm 
        v-else-if="currentForm === 'forgot'" 
        @goToLogin="currentForm = 'login'" 
      />
    </transition>
  </div>
  
  <transition name="slide-fade" mode="out-in">
    <HomeView 
      v-if="currentForm === 'home' || currentForm === 'course-mode'" 
      @selectLanguage="handleLanguageSelect" 
      @openSidebar="isSidebarOpen = true"
      @navigate="handleNavigation"
    />
  </transition>

  <transition name="slide-up">
    <LanguageCoursesView 
      v-if="currentForm === 'language-courses'" 
      :selectedLanguage="selectedLanguage"
      @goBack="currentForm = 'home'"
      @selectCourse="handleCourseSelect"
    />
  </transition>

  <transition name="slide-up">
    <CourseModeView 
      v-if="currentForm === 'course-mode'" 
      @goBack="currentForm = 'language-courses'"
      @selectMode="handleModeSelect"
    />
  </transition>

  <transition name="fade">
    <ChatView 
      v-if="currentForm === 'chat' || currentForm === 'saved-chat'" 
      :isSaved="currentForm === 'saved-chat'"
      @goBack="currentForm = 'language-courses'"
      @newPartner="currentForm = 'course-mode'"
      @continueChat="currentForm = 'home'"
    />
  </transition>

  <transition name="fade">
    <GroupChatView 
      v-if="currentForm === 'group-chat'" 
      @goBack="currentForm = 'language-courses'"
    />
  </transition>

  <transition name="fade">
    <MascotChatView 
      v-if="currentForm === 'mascot-chat'" 
      @goBack="currentForm = 'language-courses'"
      @newPartner="currentForm = 'course-mode'"
    />
  </transition>

  <transition name="fade">
    <ConversationsView 
      v-if="currentForm === 'conversations'"
      @openSidebar="isSidebarOpen = true"
      @openChat="currentForm = 'saved-chat'"
      @navigate="handleNavigation"
    />
  </transition>

  <transition name="fade">
    <ProfileView 
      v-if="currentForm === 'profile'"
      @goBack="currentForm = 'home'"
      @navigate="handleNavigation"
      @openSidebar="isSidebarOpen = true"
    />
  </transition>
</template>

<style scoped>
.slide-fade-enter-active {
  transition: all 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
}
.slide-fade-leave-active {
  transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
}
.slide-fade-enter-from {
  transform: translateY(30px) scale(0.95);
  opacity: 0;
}
.slide-fade-leave-to {
  transform: translateY(-30px) scale(0.95);
  opacity: 0;
}
.fade-enter-active,
.fade-leave-active {
  transition: all 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
}
.fade-enter-from {
  transform: translateX(100%);
  opacity: 0;
}
.fade-leave-to {
  transform: translateX(-100%);
  opacity: 0;
}

/* Slide Up transition for Modality selection */
.slide-up-enter-active,
.slide-up-leave-active {
  transition: all 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
}
.slide-up-enter-from {
  transform: translateY(100%);
  opacity: 0;
}
.slide-up-leave-to {
  transform: translateY(100%);
  opacity: 0;
}
</style>
