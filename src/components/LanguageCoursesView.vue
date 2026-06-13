<template>
  <div class="courses-container">
    <header class="header">
      <button class="back-btn" @click="$emit('goBack')">
        <ChevronLeftIcon size="24" stroke-width="2.5" />
      </button>
    </header>
    
    <div class="content-scroll">
      <div class="mascot-header">
        <img src="../assets/camaleao.png" class="peeking-mascot" />
        <h2>{{ t('select_modality') }}</h2>
        <p class="subtitle-text">{{ t('select_modality_desc') }}</p>
      </div>

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
            <div class="meta-top">
              <h3>{{ course.title }}</h3>
              <div class="course-meta">
                <span v-if="course.metaLabel && course.metaValue">
                  <UserIcon size="12" /> {{ t('age_label') }}: {{ course.metaValue }}
                </span>
                <span v-else>
                  <UserIcon size="12" /> {{ t('age_label') }}: Adultos
                </span>
              </div>
            </div>
            <button class="select-btn">{{ t('start_btn') }}</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'
import { ChevronLeftIcon, UserIcon } from '@lucide/vue'
import fallbackImg from '../assets/camaleao.png'
import { coursesData } from '../data/coursesData.js'
import { t } from '../data/translations.js'

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
  background: #ffffff;
  z-index: 40;
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

.content-scroll {
  flex: 1;
  overflow-y: auto;
  padding: 20px 16px;
}

.mascot-header {
  text-align: center;
  margin-bottom: 24px;
}

.peeking-mascot {
  height: 80px;
  object-fit: contain;
  margin-bottom: -4px;
  margin-top: -20px;
}

.mascot-header h2 {
  font-size: 24px;
  color: #1a235c;
  font-weight: 800;
  margin-bottom: 8px;
  line-height: 1.3;
}

.subtitle-text {
  color: #5c6b8a;
  font-size: 14px;
  font-weight: 500;
  margin: 0;
}

.course-cards {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
  width: 100%;
}

.course-card {
  background: white;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 4px 16px rgba(0,0,0,0.05);
  border: 1.5px solid #e2e8f0;
  cursor: pointer;
  opacity: 0;
  animation: slideUpFade 0.4s ease forwards;
  transition: transform 0.2s, box-shadow 0.2s, border-color 0.2s;
  display: flex;
  flex-direction: column;
  height: auto;
}

.course-card:hover {
  border-color: #1c5bf0;
}

.course-card:active {
  transform: scale(0.98);
}

.course-img {
  width: 100%;
  height: 190px;
  object-fit: cover;
}

.course-info {
  padding: 10px 12px 14px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  flex: 1;
  gap: 8px;
}

.meta-top {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.course-info h3 {
  margin: 0;
  font-size: 13px;
  color: #1a235c;
  font-weight: 800;
  line-height: 1.2;
}

.course-meta {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.course-meta span {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 10px;
  color: #64748b;
  font-weight: 600;
}

.select-btn {
  background: #1c5bf0;
  color: white;
  border: none;
  border-radius: 10px;
  padding: 8px 10px;
  font-weight: 700;
  font-size: 12px;
  cursor: pointer;
  transition: background 0.2s;
  width: 100%;
  text-align: center;
  margin-top: 6px;
}

.select-btn:active {
  background: #1545cc;
}
</style>
