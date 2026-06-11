<template>
  <div class="courses-container">
    <header class="header">
      <button class="back-btn" @click="$emit('goBack')">
        <ChevronLeftIcon size="24" stroke-width="2.5" />
      </button>
      <h2>Selecione um Curso</h2>
    </header>
    
    <div class="content-scroll">
      <div class="course-cards">
        <div 
          class="course-card" 
          v-for="(course, index) in currentCourses" 
          :key="course.id"
          @click="selectCourse(course.id)" 
          :style="{ animationDelay: (index * 0.1) + 's' }"
        >
          <img :src="course.image" :alt="course.title" class="course-img" @error="handleImageError($event, course.id)" />
          <div class="course-info">
            <h3>{{ course.title }}</h3>
            <div class="course-meta">
              <span><BookOpenIcon size="14" /> Material: {{ course.material }}</span>
              <span v-if="course.metaLabel && course.metaValue"><UserIcon size="14" /> {{ course.metaLabel }}: {{ course.metaValue }}</span>
            </div>
            <p>{{ course.desc }}</p>
            <button class="select-btn">Saiba Mais</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { ChevronLeftIcon, BookOpenIcon, UserIcon } from '@lucide/vue'
import fallbackImg from '../assets/header-hero.jpg'
import { coursesData } from '../data/coursesData.js'

const props = defineProps({
  selectedLanguage: {
    type: String,
    default: 'en'
  }
})

const emit = defineEmits(['goBack', 'selectCourse'])

const currentCourses = computed(() => {
  return coursesData[props.selectedLanguage] || coursesData['en']
})

const selectCourse = (courseId) => {
  emit('selectCourse', courseId)
}

const handleImageError = (event, type) => {
  event.target.src = fallbackImg
}
</script>

<style scoped>
.courses-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #f8fafc;
  z-index: 40; /* Above HomeView but below modals */
  display: flex;
  flex-direction: column;
  animation: slideIn 0.3s ease;
}

@keyframes slideIn {
  from { transform: translateX(100%); }
  to { transform: translateX(0); }
}

@keyframes slideUpFade {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.header {
  display: flex;
  align-items: center;
  padding: 16px 20px;
  background: white;
  border-bottom: 1px solid #e2e8f0;
}

.back-btn {
  background: none;
  border: none;
  color: #1a235c;
  cursor: pointer;
  padding: 8px;
  margin: -8px 16px -8px -8px;
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
}

.course-cards {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.course-card {
  background: white;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 4px 16px rgba(0,0,0,0.06);
  cursor: pointer;
  opacity: 0;
  animation: slideUpFade 0.4s ease forwards;
  transition: transform 0.2s, box-shadow 0.2s;
}

.course-card:active {
  transform: scale(0.98);
}

.course-img {
  width: 100%;
  height: 320px;
  object-fit: cover;
}

.course-info {
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.course-info h3 {
  margin: 0;
  font-size: 20px;
  color: #1a235c;
  font-weight: 800;
}

.course-meta {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.course-meta span {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 13px;
  color: #64748b;
  font-weight: 600;
}

.course-info p {
  margin: 8px 0 16px 0;
  font-size: 14px;
  color: #475569;
  line-height: 1.5;
}

.select-btn {
  background: #1c5bf0;
  color: white;
  border: none;
  border-radius: 12px;
  padding: 12px;
  font-weight: 700;
  font-size: 15px;
  cursor: pointer;
  transition: background 0.2s;
  width: 100%;
}

.select-btn:active {
  background: #1545cc;
}
</style>
