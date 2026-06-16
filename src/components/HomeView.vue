<template>
  <div class="home-container">
    <!-- Static App Header -->
    <header class="app-header">
      <div class="header-logo-container" @click="$emit('navigate', 'home')" style="cursor: pointer;">
        <img src="../assets/img/logo-change.png" class="header-logo" alt="Change" />
      </div>
      <h2 class="header-title">{{ t('nav_home') }}</h2>
      <button class="menu-btn" @click="$emit('openSidebar')">
        <MenuIcon size="24" stroke-width="2.5" />
      </button>
    </header>

    <div class="home-content">
      <div class="mascot-header">
        <img src="../assets/camaleao.png" class="peeking-mascot" />
        <h2>{{ t('home_title') }}</h2>
        <p>{{ t('home_subtitle') }}</p>
      </div>
      
      <div class="cards-grid">
        <div class="lang-card" @click="$emit('selectLanguage', 'en')" style="animation-delay: 0.15s">
          <img src="../assets/englishcard.jpeg" alt="Inglês" />
          <div class="flag-circle"><img src="https://flagcdn.com/w40/gb.png" alt="UK Flag" /></div>
          <div class="card-label" style="background-color: #21448b;">Inglês</div>
        </div>
        <div class="lang-card" @click="$emit('selectLanguage', 'es')" style="animation-delay: 0.3s">
          <img src="../assets/espanolcard.jpeg" alt="Espanhol" />
          <div class="flag-circle"><img src="https://flagcdn.com/w40/es.png" alt="Spain Flag" /></div>
          <div class="card-label" style="background-color: #b05716;">Espanhol</div>
        </div>
        <div class="lang-card" @click="$emit('selectLanguage', 'fr')" style="animation-delay: 0.45s">
          <img src="../assets/francescard.jpeg" alt="Francês" />
          <div class="flag-circle"><img src="https://flagcdn.com/w40/fr.png" alt="France Flag" /></div>
          <div class="card-label" style="background-color: #28446b;">Francês</div>
        </div>
        <div class="lang-card" @click="$emit('selectLanguage', 'pt')" style="animation-delay: 0.6s">
          <img src="../assets/portuguesecard.jpeg" alt="Português" />
          <div class="flag-circle"><img src="https://flagcdn.com/w40/br.png" alt="Brazil Flag" /></div>
          <div class="card-label" style="background-color: #0b5e28;">Português</div>
        </div>
      </div>

      <!-- TESTIMONIALS SECTION -->
      <div class="testimonials-section">
        <!-- Row 1: Left sliding -->
        <div class="marquee-container">
          <div class="marquee-track marquee-left">
            <div class="marquee-content">
              <div v-for="(item, index) in row1Testimonials" :key="'r1-' + index" class="testimonial-card">
                <div class="avatar-container">
                  <img :src="item.avatar" class="avatar-img" />
                  <img :src="item.flag" class="flag-badge" />
                </div>
                <div class="speech-bubble" :style="{ backgroundColor: item.bgColor }">
                  <div class="speech-pointer" :style="{ borderRightColor: item.bgColor }"></div>
                  <div class="speech-text">{{ item.text }}</div>
                  <div class="speech-footer">
                    <span class="speech-author">{{ item.author }}</span>
                    <div v-if="item.trustpilot" class="trustpilot-logo">
                      <svg class="tp-star" viewBox="0 0 24 24" width="12" height="12" fill="#00b67a">
                        <path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/>
                      </svg>
                      <span>Trustpilot</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!-- Duplicate content for seamless marquee loop -->
            <div class="marquee-content" aria-hidden="true">
              <div v-for="(item, index) in row1Testimonials" :key="'r1-dup-' + index" class="testimonial-card">
                <div class="avatar-container">
                  <img :src="item.avatar" class="avatar-img" />
                  <img :src="item.flag" class="flag-badge" />
                </div>
                <div class="speech-bubble" :style="{ backgroundColor: item.bgColor }">
                  <div class="speech-pointer" :style="{ borderRightColor: item.bgColor }"></div>
                  <div class="speech-text">{{ item.text }}</div>
                  <div class="speech-footer">
                    <span class="speech-author">{{ item.author }}</span>
                    <div v-if="item.trustpilot" class="trustpilot-logo">
                      <svg class="tp-star" viewBox="0 0 24 24" width="12" height="12" fill="#00b67a">
                        <path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/>
                      </svg>
                      <span>Trustpilot</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Row 2: Right sliding -->
        <div class="marquee-container">
          <div class="marquee-track marquee-right">
            <div class="marquee-content">
              <div v-for="(item, index) in row2Testimonials" :key="'r2-' + index" class="testimonial-card">
                <div class="avatar-container">
                  <img :src="item.avatar" class="avatar-img" />
                  <img :src="item.flag" class="flag-badge" />
                </div>
                <div class="speech-bubble" :style="{ backgroundColor: item.bgColor }">
                  <div class="speech-pointer" :style="{ borderRightColor: item.bgColor }"></div>
                  <div class="speech-text">{{ item.text }}</div>
                  <div class="speech-footer">
                    <span class="speech-author">{{ item.author }}</span>
                    <div v-if="item.trustpilot" class="trustpilot-logo">
                      <svg class="tp-star" viewBox="0 0 24 24" width="12" height="12" fill="#00b67a">
                        <path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/>
                      </svg>
                      <span>Trustpilot</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!-- Duplicate content for seamless marquee loop -->
            <div class="marquee-content" aria-hidden="true">
              <div v-for="(item, index) in row2Testimonials" :key="'r2-dup-' + index" class="testimonial-card">
                <div class="avatar-container">
                  <img :src="item.avatar" class="avatar-img" />
                  <img :src="item.flag" class="flag-badge" />
                </div>
                <div class="speech-bubble" :style="{ backgroundColor: item.bgColor }">
                  <div class="speech-pointer" :style="{ borderRightColor: item.bgColor }"></div>
                  <div class="speech-text">{{ item.text }}</div>
                  <div class="speech-footer">
                    <span class="speech-author">{{ item.author }}</span>
                    <div v-if="item.trustpilot" class="trustpilot-logo">
                      <svg class="tp-star" viewBox="0 0 24 24" width="12" height="12" fill="#00b67a">
                        <path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/>
                      </svg>
                      <span>Trustpilot</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Middle Title -->
        <h3 class="testimonials-title">Nossos usuários, nos bastidores.</h3>

        <!-- Row 3: Left sliding -->
        <div class="marquee-container">
          <div class="marquee-track marquee-left">
            <div class="marquee-content">
              <div v-for="(item, index) in row3Testimonials" :key="'r3-' + index" class="testimonial-card">
                <div class="avatar-container">
                  <img :src="item.avatar" class="avatar-img" />
                  <img :src="item.flag" class="flag-badge" />
                </div>
                <div class="speech-bubble" :style="{ backgroundColor: item.bgColor }">
                  <div class="speech-pointer" :style="{ borderRightColor: item.bgColor }"></div>
                  <div class="speech-text">{{ item.text }}</div>
                  <div class="speech-footer">
                    <span class="speech-author">{{ item.author }}</span>
                    <div v-if="item.trustpilot" class="trustpilot-logo">
                      <svg class="tp-star" viewBox="0 0 24 24" width="12" height="12" fill="#00b67a">
                        <path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/>
                      </svg>
                      <span>Trustpilot</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!-- Duplicate content for seamless marquee loop -->
            <div class="marquee-content" aria-hidden="true">
              <div v-for="(item, index) in row3Testimonials" :key="'r3-dup-' + index" class="testimonial-card">
                <div class="avatar-container">
                  <img :src="item.avatar" class="avatar-img" />
                  <img :src="item.flag" class="flag-badge" />
                </div>
                <div class="speech-bubble" :style="{ backgroundColor: item.bgColor }">
                  <div class="speech-pointer" :style="{ borderRightColor: item.bgColor }"></div>
                  <div class="speech-text">{{ item.text }}</div>
                  <div class="speech-footer">
                    <span class="speech-author">{{ item.author }}</span>
                    <div v-if="item.trustpilot" class="trustpilot-logo">
                      <svg class="tp-star" viewBox="0 0 24 24" width="12" height="12" fill="#00b67a">
                        <path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/>
                      </svg>
                      <span>Trustpilot</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Row 4: Right sliding -->
        <div class="marquee-container">
          <div class="marquee-track marquee-right">
            <div class="marquee-content">
              <div v-for="(item, index) in row4Testimonials" :key="'r4-' + index" class="testimonial-card">
                <div class="avatar-container">
                  <img :src="item.avatar" class="avatar-img" />
                  <img :src="item.flag" class="flag-badge" />
                </div>
                <div class="speech-bubble" :style="{ backgroundColor: item.bgColor }">
                  <div class="speech-pointer" :style="{ borderRightColor: item.bgColor }"></div>
                  <div class="speech-text">{{ item.text }}</div>
                  <div class="speech-footer">
                    <span class="speech-author">{{ item.author }}</span>
                    <div v-if="item.trustpilot" class="trustpilot-logo">
                      <svg class="tp-star" viewBox="0 0 24 24" width="12" height="12" fill="#00b67a">
                        <path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/>
                      </svg>
                      <span>Trustpilot</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <!-- Duplicate content for seamless marquee loop -->
            <div class="marquee-content" aria-hidden="true">
              <div v-for="(item, index) in row4Testimonials" :key="'r4-dup-' + index" class="testimonial-card">
                <div class="avatar-container">
                  <img :src="item.avatar" class="avatar-img" />
                  <img :src="item.flag" class="flag-badge" />
                </div>
                <div class="speech-bubble" :style="{ backgroundColor: item.bgColor }">
                  <div class="speech-pointer" :style="{ borderRightColor: item.bgColor }"></div>
                  <div class="speech-text">{{ item.text }}</div>
                  <div class="speech-footer">
                    <span class="speech-author">{{ item.author }}</span>
                    <div v-if="item.trustpilot" class="trustpilot-logo">
                      <svg class="tp-star" viewBox="0 0 24 24" width="12" height="12" fill="#00b67a">
                        <path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/>
                      </svg>
                      <span>Trustpilot</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- SUPPORT/SCHOOLS SECTION -->
      <div class="support-section">
        <div class="support-card">
          <h3 class="support-title">Trabalhamos com centenas de escolas e bibliotecas.</h3>
          <p class="support-desc">
            A Change Skills oferece descontos especiais em nossa plataforma de aprendizado de idiomas para espaços educacionais e comunitários.
          </p>
          
          <div class="support-image-container">
            <img src="https://lingopie.com/cdn-cgi/image/format=auto,quality=75,fit=scale-down/https://d1ndg56vpa7k9f.cloudfront.net/assets/homepage/schools_mobile_c.png" class="support-image" alt="Escolas e bibliotecas" />
            <button class="support-btn" @click="handleSupportClick">Contate-nos</button>
          </div>
        </div>
      </div>

    </div>
    <nav class="bottom-nav">
      <div class="nav-item active" @click="$emit('navigate', 'home')">
        <HomeIcon size="28" />
        <span>{{ t('nav_home') }}</span>
      </div>
      <div class="nav-item" @click="$emit('navigate', 'conversations')">
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
import { 
  Home as HomeIcon, 
  MessageCircle as MessageCircleIcon, 
  User as UserIcon,
  Menu as MenuIcon
} from '@lucide/vue'
import { t } from '../data/translations.js'

defineEmits(['selectLanguage', 'openSidebar', 'navigate'])

const row1Testimonials = [
  {
    avatar: 'https://images.unsplash.com/photo-1544005313-94ddf0286df2?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/fr.png',
    text: 'I learned so much!',
    author: 'Orian, FR',
    bgColor: '#ff8541',
    trustpilot: false
  },
  {
    avatar: 'https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/us.png',
    text: 'Oh my goodness, I look forward to this!',
    author: 'Jasmine, CA',
    bgColor: '#d8b4fe',
    trustpilot: true
  },
  {
    avatar: 'https://images.unsplash.com/photo-1494790108377-be9c29b29330?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/es.png',
    text: '¡Me encanta practicar conversación!',
    author: 'Sofia, ES',
    bgColor: '#6ee7b7',
    trustpilot: false
  },
  {
    avatar: 'https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/be.png',
    text: 'J\'adore discuter with the chameleon!',
    author: 'Antoine, BE',
    bgColor: '#fbcfe8',
    trustpilot: true
  }
]

const row2Testimonials = [
  {
    avatar: 'https://images.unsplash.com/photo-1500648767791-00dcc994a43e?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/br.png',
    text: 'Muito mais prático do que qualquer cursinho.',
    author: 'Lucas, BR',
    bgColor: '#d9f99d',
    trustpilot: false
  },
  {
    avatar: 'https://images.unsplash.com/photo-1438761681033-6461ffad8d80?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/gb.png',
    text: 'Best language app I\'ve ever used, hands down.',
    author: 'Emma, UK',
    bgColor: '#93c5fd',
    trustpilot: true
  },
  {
    avatar: 'https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/de.png',
    text: 'Die Gruppenchats sind einfach genial!',
    author: 'Hans, DE',
    bgColor: '#ff8541',
    trustpilot: false
  },
  {
    avatar: 'https://images.unsplash.com/photo-1517841905240-472988babdf9?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/br.png',
    text: 'O camaleão de IA é super divertido!',
    author: 'Mariana, BR',
    bgColor: '#d8b4fe',
    trustpilot: true
  }
]

const row3Testimonials = [
  {
    avatar: 'https://images.unsplash.com/photo-1508214751196-bcfd4ca60f91?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/pt.png',
    text: 'Absolutely great. I am enjoying it a lot!',
    author: 'Maria, PT',
    bgColor: '#d8b4fe',
    trustpilot: true
  },
  {
    avatar: 'https://images.unsplash.com/photo-1522075469751-3a6694fb2f61?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/ca.png',
    text: 'The UI is extremely fluid and clean.',
    author: 'Dave, CA',
    bgColor: '#6ee7b7',
    trustpilot: false
  },
  {
    avatar: 'https://images.unsplash.com/photo-1506794778202-cad84cf45f1d?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/mx.png',
    text: '¡Las rachas de días me motivan un montón!',
    author: 'Diego, MX',
    bgColor: '#d9f99d',
    trustpilot: true
  },
  {
    avatar: 'https://images.unsplash.com/photo-1531746020798-e6953c6e8e04?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/fr.png',
    text: 'Une excellente méthode pour s\'exercer.',
    author: 'Chloé, FR',
    bgColor: '#fbcfe8',
    trustpilot: false
  }
]

const row4Testimonials = [
  {
    avatar: 'https://images.unsplash.com/photo-1539571696357-5a69c17a67c6?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/br.png',
    text: 'Nunca foi tão divertido aprender inglês!',
    author: 'Felipe, BR',
    bgColor: '#ff8541',
    trustpilot: true
  },
  {
    avatar: 'https://images.unsplash.com/photo-1524504388940-b1c1722653e1?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/us.png',
    text: 'So much better than other gamified apps.',
    author: 'Sarah, US',
    bgColor: '#93c5fd',
    trustpilot: false
  },
  {
    avatar: 'https://images.unsplash.com/photo-1501196354995-cbb51c65aaea?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/es.png',
    text: 'Increíble experiencia de conversación.',
    author: 'Carlos, ES',
    bgColor: '#d9f99d',
    trustpilot: true
  },
  {
    avatar: 'https://images.unsplash.com/photo-1554151228-14d9def656e4?auto=format&fit=crop&w=150&h=150&q=80',
    flag: 'https://flagcdn.com/w40/be.png',
    text: 'Super pratique et ludique.',
    author: 'Camille, BE',
    bgColor: '#fbcfe8',
    trustpilot: false
  }
]

const handleSupportClick = () => {
  alert('Iniciando atendimento de suporte para escolas e bibliotecas...')
}
</script>

<style scoped>
.home-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: radial-gradient(circle at top right, #f4f8ff 0%, #ffffff 45%);
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
  from { 
    opacity: 0; 
    transform: translateY(35px) scale(0.92) rotate(-1deg); 
  }
  to { 
    opacity: 1; 
    transform: translateY(0) scale(1) rotate(0); 
  }
}

/* Header styling */
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

.home-content {
  flex: 1;
  padding: 24px;
  overflow-y: auto;
  padding-bottom: 100px; /* space for bottom nav */
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

@keyframes floatMascot {
  0% { transform: translateY(0) rotate(0deg); }
  50% { transform: translateY(-4px) rotate(1.5deg); }
  100% { transform: translateY(0) rotate(0deg); }
}

.mascot-header {
  text-align: center;
  margin-top: 20px;
  margin-bottom: 36px;
}

.peeking-mascot {
  height: 80px;
  object-fit: contain;
  margin-bottom: -4px;
  margin-top: -20px;
  animation: floatMascot 4s ease-in-out infinite;
}

.mascot-header h2 {
  font-size: 26px;
  background: linear-gradient(135deg, #1a235c 0%, #1c5bf0 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: 900;
  margin-bottom: 10px;
  line-height: 1.35;
  letter-spacing: -0.5px;
}

.mascot-header p {
  color: #5c6b8a;
  font-size: 15.5px;
  font-weight: 500;
  opacity: 0.9;
}

.cards-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 16px;
  width: 100%;
  max-width: 400px;
  margin-bottom: 48px; /* increased section spacing */
}

.lang-card {
  position: relative;
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
  height: 240px;
  cursor: pointer;
  transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1), box-shadow 0.4s ease;
  opacity: 0;
  animation: slideUpFade 1.1s cubic-bezier(0.34, 1.56, 0.64, 1) forwards; /* slowed down animation speed */
}

/* Card Hover effect */
.lang-card:hover {
  transform: translateY(-8px) scale(1.03);
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.15);
}

/* Card Glare Shine Effect */
.lang-card::after {
  content: '';
  position: absolute;
  top: 0;
  left: -150%;
  width: 125%;
  height: 100%;
  background: linear-gradient(
    to right,
    rgba(255, 255, 255, 0) 0%,
    rgba(255, 255, 255, 0.3) 50%,
    rgba(255, 255, 255, 0) 100%
  );
  transform: skewX(-25deg);
  transition: 0.75s;
}

.lang-card:hover::after {
  left: 150%;
}

.lang-card:active {
  transform: scale(0.97);
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

.lang-card img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s cubic-bezier(0.25, 1, 0.5, 1);
}

.lang-card:hover img {
  transform: scale(1.08);
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
  transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.lang-card:hover .flag-circle {
  transform: scale(1.15) rotate(8deg);
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
  transition: padding-bottom 0.3s ease, font-size 0.3s ease;
}

.lang-card:hover .card-label {
  padding-bottom: 16px;
  font-size: 17px;
}

/* TESTIMONIALS STYLES */
.testimonials-section {
  width: 100%;
  margin-top: 24px;
  margin-bottom: 40px; /* increased section spacing */
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.testimonials-title {
  text-align: center;
  font-size: 24px;
  font-weight: 850;
  color: #111111;
  margin: 18px 0;
  line-height: 1.2;
  font-family: 'Inter', sans-serif;
  letter-spacing: -0.5px;
}

.marquee-container {
  overflow: hidden;
  width: 100%;
  padding: 4px 0;
}

.marquee-track {
  display: flex;
  width: max-content;
}

.marquee-content {
  display: flex;
  gap: 20px;
  padding-right: 20px;
}

@keyframes marquee-l {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-50%);
  }
}

@keyframes marquee-r {
  0% {
    transform: translateX(-50%);
  }
  100% {
    transform: translateX(0);
  }
}

.marquee-left {
  animation: marquee-l 58s linear infinite; /* slowed down scroll marquee */
}

.marquee-right {
  animation: marquee-r 58s linear infinite; /* slowed down scroll marquee */
}

/* Pause scroll on hover for better user experience */
.marquee-track:hover {
  animation-play-state: paused;
}

.testimonial-card {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-shrink: 0;
}

.avatar-container {
  position: relative;
  width: 48px;
  height: 48px;
  flex-shrink: 0;
}

.avatar-img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: cover;
  border: 2px solid #fff;
  box-shadow: 0 3px 8px rgba(0,0,0,0.1);
}

.flag-badge {
  position: absolute;
  bottom: -2px;
  right: -2px;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  object-fit: cover;
  border: 1.5px solid #fff;
  box-shadow: 0 1px 4px rgba(0,0,0,0.25);
}

.speech-bubble {
  position: relative;
  padding: 12px 16px;
  border-radius: 18px;
  border-bottom-left-radius: 4px; /* points to avatar */
  box-shadow: 0 4px 10px rgba(0,0,0,0.05);
  color: #111;
  font-size: 13px;
  font-weight: 600;
  display: flex;
  flex-direction: column;
  gap: 4px;
  max-width: 250px;
  white-space: normal;
}

.speech-pointer {
  position: absolute;
  left: -6px;
  bottom: 8px;
  width: 0;
  height: 0;
  border-top: 5px solid transparent;
  border-bottom: 5px solid transparent;
  border-right: 6px solid; /* color bound dynamically */
}

.speech-text {
  font-weight: 750;
  font-size: 13.5px;
  line-height: 1.3;
}

.speech-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
  margin-top: 2px;
}

.speech-author {
  font-size: 11px;
  color: #333333;
  font-weight: 650;
}

.trustpilot-logo {
  display: flex;
  align-items: center;
  gap: 3px;
  font-size: 10px;
  color: #222;
  font-weight: 700;
}

/* SUPPORT/SCHOOLS SECTION STYLES */
.support-section {
  width: 100%;
  max-width: 400px;
  margin-top: 24px;
  margin-bottom: 48px; /* spacing at bottom */
  padding: 0 12px;
  opacity: 0;
  animation: slideUpFade 1.1s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
  animation-delay: 0.8s;
}

.support-card {
  background-color: #1c5bf0; /* brand blue */
  border-radius: 32px;
  padding: 40px 24px 0 24px; /* image fills the bottom */
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  overflow: hidden;
  position: relative;
  box-shadow: 0 8px 24px rgba(28, 91, 240, 0.25);
}

.support-title {
  font-size: 22px;
  font-weight: 800;
  color: #ffffff; /* white text */
  line-height: 1.3;
  margin-bottom: 16px;
  font-family: 'Inter', sans-serif;
}

.support-desc {
  font-size: 14.5px;
  font-weight: 500;
  color: #ffffff; /* white text */
  line-height: 1.5;
  margin-bottom: 36px;
  opacity: 0.95;
}

.support-image-container {
  width: calc(100% + 48px);
  margin-left: -24px;
  margin-right: -24px;
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.support-image {
  width: 100%;
  height: auto;
  display: block;
  object-fit: cover;
  filter: saturate(1.05) contrast(1.02);
}

.support-btn {
  position: absolute;
  bottom: 24px;
  background: #ffffff;
  color: #1c5bf0; /* brand blue text */
  border: none;
  border-radius: 30px;
  padding: 14px 32px;
  font-size: 16px;
  font-weight: 750;
  cursor: pointer;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
  transition: transform 0.2s, box-shadow 0.2s;
}

.support-btn:hover {
  transform: scale(1.05);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.2);
}

.support-btn:active {
  transform: scale(0.95);
}

.bottom-nav {
  display: flex;
  justify-content: space-around;
  align-items: center; /* Center items vertically */
  height: 72px; /* Fixed height for vertical centering */
  background: white;
  border-top: 1px solid #e2e8f0;
  padding-bottom: env(safe-area-inset-bottom, 0px);
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
