<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import BlossomEffect from './components/BlossomEffect.vue'; // Import effect

// Helper to resolve paths relative to base URL (for deployment)
const resolvePath = (path) => {
  return new URL(path, import.meta.url).href;
}

// Or more simply, since these are in public folder, just prepend BASE_URL
const basePath = import.meta.env.BASE_URL;

// Generate image paths (photo_01.jpg to photo_16.jpg)
const images = Array.from({ length: 16 }, (_, i) => 
  `${basePath}imgs/photo_${String(i + 1).padStart(2, '0')}.jpg`
);

const currentSlide = ref(0);
let slideInterval;

const nextSlide = () => {
  currentSlide.value = (currentSlide.value + 1) % images.length;
};

const prevSlide = () => {
  currentSlide.value = (currentSlide.value - 1 + images.length) % images.length;
};

const startSlideShow = () => {
  slideInterval = setInterval(nextSlide, 3000);
};

const stopSlideShow = () => {
  clearInterval(slideInterval);
};

onMounted(() => {
  startSlideShow();
});

onUnmounted(() => {
  stopSlideShow();
});

// Language State
const currentLang = ref('ko'); // 'ko' or 'en'

const translations = {
  ko: {
    subtitle: "소중한 날에 초대합니다",
    greetingTitle: "소중한 분들을 초대합니다",
    greetingMessage: "두 사람이 하나가 되는 뜻깊은 날,<br>귀한 걸음 하시어 축복해 주시면<br>더없는 기쁨으로 간직하겠습니다.",
    dateLabel: "Date",
    dateValue: "2026. 03. 28. 토요일 오후 5:40",
    locationLabel: "Location",
    locationValue: "대전광역시 유성구 <br>테크노중앙로 161<br>스카이파크호텔 1층<br> 루이비스컨벤션",
    locationCopy: "대전광역시 유성구 테크노중앙로 161",
    accountTitle: "마음 전하실 곳",
    accountMessage: "참석하지 못하시는 분들을 위해<br>계좌번호를 안내해 드립니다.<br>너른 양해 부탁드립니다.",
    groomSide: "신랑측",
    brideSide: "신부측",
    groomName: "조준희",
    brideName: "신인숙",
    groomBank: "국민은행",
    brideBank: "우리은행",
    copyAddress: "주소 복사",
    copyAccount: "계좌번호 복사",
    thankYou: "감사합니다"
  },
  en: {
    subtitle: "We invite you to our special day",
    greetingTitle: "We Invite You",
    greetingMessage: "We invite you to celebrate<br>the beginning of our new journey together.<br>Your presence will add to our joy.",
    dateLabel: "Date",
    dateValue: "Saturday, March 28, 2026 at 5:40 PM",
    locationLabel: "Location",
    locationValue: "1F Louivise Convention,<br>Skypark Hotel, 161 Techno jungang-ro,<br>Yuseong-gu, Daejeon",
    locationCopy: "161 Techno jungang-ro, Yuseong-gu, Daejeon",
    accountTitle: "Gift Information",
    accountMessage: "For those who wish to send a gift,<br>account details are provided below.",
    groomSide: "Groom",
    brideSide: "Bride",
    groomName: "Cho Jun-hee",
    brideName: "Shin In-sook",
    groomBank: "KB Bank",
    brideBank: "Woori Bank",
    copyAddress: "Copy Address",
    copyAccount: "Copy Account",
    thankYou: "Thank you"
  }
};

const toggleLanguage = () => {
  currentLang.value = currentLang.value === 'ko' ? 'en' : 'ko';
};

const copyToClipboard = (text) => {
  navigator.clipboard.writeText(text).then(() => {
    alert(currentLang.value === 'ko' ? '복사되었습니다: ' + text : 'Copied: ' + text);
  }).catch(err => {
    console.error('Copy failed:', err);
    alert(currentLang.value === 'ko' ? '복사하기가 지원되지 않는 환경입니다.' : 'Copying is not supported in this environment.');
  });
};
</script>

<template>
  <div class="mobile-container">
    <BlossomEffect />
    <div class="lang-switch-container">
      <button class="lang-btn" @click="toggleLanguage">
        {{ currentLang === 'ko' ? 'English' : '한국어' }}
      </button>
    </div>
    <header class="header">
      <h1 class="title">Wedding Invitation</h1>
      <p class="subtitle">{{ translations[currentLang].subtitle }}</p>
    </header>

    <main class="content">
      <section class="video-section">
        <video controls autoplay muted loops playsinline src="/weddingVideo.mp4"></video>
      </section>

      <section class="gallery">
        <div class="slider-container">
          <div class="slide-wrapper" :style="{ transform: `translateX(-${currentSlide * 100}%)` }">
            <div 
              v-for="(img, index) in images" 
              :key="index" 
              class="slide"
            >
              <img :src="img" alt="Wedding Photo" loading="lazy" />
            </div>
          </div>
          
          <button class="nav-btn prev" @click="prevSlide">&#10094;</button>
          <button class="nav-btn next" @click="nextSlide">&#10095;</button>

          <div class="dots">
            <span 
              v-for="(img, index) in images" 
              :key="'dot-' + index" 
              class="dot" 
              :class="{ active: currentSlide === index }"
              @click="currentSlide = index"
            ></span>
          </div>
        </div>
      </section>

      <section class="greeting">
        <h2>{{ translations[currentLang].greetingTitle }}</h2>
        <p class="message" v-html="translations[currentLang].greetingMessage"></p>
      </section>


      <section class="details">
        <div class="info-item">
          <span class="label">{{ translations[currentLang].dateLabel }}</span>
          <span class="value">{{ translations[currentLang].dateValue }}</span>
        </div>
        <div class="info-item">
          <span class="label">{{ translations[currentLang].locationLabel }}</span>
          <span class="value" v-html="translations[currentLang].locationValue"></span>
          <button class="copy-btn" @click="copyToClipboard(translations[currentLang].locationCopy)" :aria-label="translations[currentLang].copyAddress">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" viewBox="0 0 16 16">
              <path d="M4 1.5H3a2 2 0 0 0-2 2V14a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V3.5a2 2 0 0 0-2-2h-1v1h1a1 1 0 0 1 1 1V14a1 1 0 0 1-1 1H3a1 1 0 0 1-1-1V3.5a1 1 0 0 1 1-1h1v-1z"/>
              <path d="M9.5 1a.5.5 0 0 1 .5.5v1a.5.5 0 0 1-.5.5h-3a.5.5 0 0 1-.5-.5v-1a.5.5 0 0 1 .5-.5h3zm-3-1A1.5 1.5 0 0 0 5 1.5v1A1.5 1.5 0 0 0 6.5 4h3A1.5 1.5 0 0 0 11 2.5v-1A1.5 1.5 0 0 0 9.5 0h-3z"/>
            </svg>
          </button>
        </div>
      </section>


      <section class="account-section">
        <h3>{{ translations[currentLang].accountTitle }}</h3>
        <p class="account-message" v-html="translations[currentLang].accountMessage"></p>
        
        <div class="account-box">
          <div class="account-group">
            <span class="side-label">{{ translations[currentLang].groomSide }}</span>
            <div class="account-info">
              <span class="name">{{ translations[currentLang].groomName }}</span>
              <div class="bank-row">
                <span class="bank">{{ translations[currentLang].groomBank }} <br>451-21-0312-080</span>
                <button class="copy-btn-sm" @click="copyToClipboard('451-21-0312-080')" :aria-label="translations[currentLang].copyAccount">
                  <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" fill="currentColor" viewBox="0 0 16 16">
                    <path d="M4 1.5H3a2 2 0 0 0-2 2V14a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V3.5a2 2 0 0 0-2-2h-1v1h1a1 1 0 0 1 1 1V14a1 1 0 0 1-1 1H3a1 1 0 0 1-1-1V3.5a1 1 0 0 1 1-1h1v-1z"/>
                    <path d="M9.5 1a.5.5 0 0 1 .5.5v1a.5.5 0 0 1-.5.5h-3a.5.5 0 0 1-.5-.5v-1a.5.5 0 0 1 .5-.5h3zm-3-1A1.5 1.5 0 0 0 5 1.5v1A1.5 1.5 0 0 0 6.5 4h3A1.5 1.5 0 0 0 11 2.5v-1A1.5 1.5 0 0 0 9.5 0h-3z"/>
                  </svg>
                </button>
              </div>
            </div>
          </div>
          
          <div class="divider"></div>

          <div class="account-group">
            <span class="side-label">{{ translations[currentLang].brideSide }}</span>
            <div class="account-info">
              <span class="name">{{ translations[currentLang].brideName }}</span>
              <div class="bank-row">
                <span class="bank">{{ translations[currentLang].brideBank }} <br>388-021774-12-001</span>
                <button class="copy-btn-sm" @click="copyToClipboard('388-021774-12-001')" :aria-label="translations[currentLang].copyAccount">
                  <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" fill="currentColor" viewBox="0 0 16 16">
                    <path d="M4 1.5H3a2 2 0 0 0-2 2V14a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V3.5a2 2 0 0 0-2-2h-1v1h1a1 1 0 0 1 1 1V14a1 1 0 0 1-1 1H3a1 1 0 0 1-1-1V3.5a1 1 0 0 1 1-1h1v-1z"/>
                    <path d="M9.5 1a.5.5 0 0 1 .5.5v1a.5.5 0 0 1-.5.5h-3a.5.5 0 0 1-.5-.5v-1a.5.5 0 0 1 .5-.5h3zm-3-1A1.5 1.5 0 0 0 5 1.5v1A1.5 1.5 0 0 0 6.5 4h3A1.5 1.5 0 0 0 11 2.5v-1A1.5 1.5 0 0 0 9.5 0h-3z"/>
                  </svg>
                </button>
              </div>
            </div>
          </div>
        </div>
      </section>

    </main>

    <footer class="footer">
      <p>{{ translations[currentLang].thankYou }}</p>
    </footer>
  </div>
</template>

<style scoped>
.mobile-container {
  width: 100%;
  max-width: 480px; /* Mobile width simulation on desktop */
  background-color: #fff;
  min-height: 100vh;
  box-shadow: 0 0 20px rgba(0,0,0,0.05); /* Subtle shadow for desktop view */
  display: flex;
  flex-direction: column;
}

.header {
  padding: 3rem 1.5rem;
  text-align: center;
  background-color: #fcfcfc;
  position: relative; /* For lang btn positioning */
}

.lang-switch-container {
  display: flex;
  justify-content: flex-end;
  padding: 1rem 1.5rem 0;
  background-color: #fcfcfc;
}

.lang-btn {
  background-color: transparent;
  border: 1px solid var(--pastel-pink-dark);
  color: var(--pastel-pink-dark);
  padding: 0.3rem 0.8rem;
  border-radius: 20px;
  font-family: 'Noto Serif KR', serif;
  font-size: 0.85rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.lang-btn:hover {
  background-color: var(--pastel-pink);
  color: white;
  border-color: var(--pastel-pink);
}

.title {
  font-family: 'Great Vibes', cursive;
  font-size: 3.5rem;
  color: var(--pastel-pink-dark); /* Dark pastel pink */
  margin-bottom: 0.5rem;
}

.subtitle {
  font-size: 1rem;
  color: var(--text-light); /* Softer text color */
  letter-spacing: 2px;
  text-transform: uppercase;
}

.content {
  flex: 1;
  padding: 2rem 1.5rem;
  background-color: var(--pastel-cream); /* Spring pastel background */
}

.greeting {
  text-align: center;
  margin-bottom: 3rem;
}

.greeting h2 {
  font-size: 1.4rem;
  margin-bottom: 1.5rem;
  color: var(--text-color);
}

.message {
  font-size: 1.1rem;
  line-height: 2;
  color: var(--text-color);
}



.gallery {
  background-color: rgba(255, 255, 255, 0.8);
  padding: 2rem; /* Add padding to frame the photos */
  border-radius: 20px; /* Rounded corners */
  border: 2px solid var(--pastel-pink); /* Pretty border */
  margin-bottom: 3rem;
  box-shadow: 0 4px 15px rgba(255, 183, 197, 0.2);
  backdrop-filter: blur(5px);
}

.slider-container {
  position: relative;
  width: 100%;
  overflow: hidden; /* Hide overflow images */
  aspect-ratio: 3/4; /* Adjust aspect ratio as needed */
  background-color: #f0f0f0;
  border-radius: 12px; /* Soften internal image corners */
}

.slide-wrapper {
  display: flex;
  transition: transform 0.5s ease-in-out;
  height: 100%;
}

.slide {
  min-width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.slide img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* or contain, depending on preference */
}

.nav-btn {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(0,0,0,0.3);
  color: white;
  border: none;
  padding: 1rem 0.5rem;
  cursor: pointer;
  z-index: 10;
  font-size: 1.2rem;
}

.prev { left: 0; }
.next { right: 0; }

.dots {
  position: absolute;
  bottom: 10px;
  width: 100%;
  text-align: center;
  z-index: 10;
}

.dot {
  display: inline-block;
  width: 8px;
  height: 8px;
  background-color: rgba(255,255,255,0.5);
  border-radius: 50%;
  margin: 0 4px;
  cursor: pointer;
}

.dot.active {
  background-color: white;
}

.video-section {
  background-color: rgba(255, 255, 255, 0.8);
  padding: 2rem;
  border-radius: 20px; /* Rounded corners */
  border: 2px solid var(--pastel-pink); /* Pretty border */
  margin-bottom: 3rem;
  box-shadow: 0 4px 15px rgba(255, 183, 197, 0.2);
  backdrop-filter: blur(5px);
}

.video-section video {
  width: 100%;
  display: block;
  border-radius: 12px; /* Soften video corners */
}


.details {
  background-color: rgba(255, 255, 255, 0.8);
  padding: 2rem;
  border-radius: 20px; /* Rounded corners */
  border: 2px solid var(--pastel-pink); /* Pretty border */
  text-align: center;
  margin-bottom: 3rem;
  box-shadow: 0 4px 15px rgba(255, 183, 197, 0.2);
}

.account-section {
  text-align: center;
  margin-bottom: 3rem;
  /* Removed padding to match alignment */
}

.account-section h3 {
  font-family: 'Noto Serif KR', serif;
  font-size: 1.3rem;
  color: var(--pastel-pink-dark);
  margin-bottom: 1rem;
  position: relative;
  display: inline-block;
  padding: 0.5rem 1.5rem;
  background-color: var(--pastel-cream);
  border-radius: 20px;
  border: 1px solid var(--pastel-pink);
}

.account-message {
  font-size: 0.95rem;
  color: #666;
  margin-bottom: 2rem;
  line-height: 1.8;
}

.account-box {
  background-color: rgba(255, 255, 255, 0.8);
  border: 2px solid var(--pastel-pink); /* Pretty border */
  border-radius: 20px; /* Rounded corners */
  padding: 2rem; /* Consistent padding with details */
  box-shadow: 0 4px 15px rgba(255, 183, 197, 0.2);
  backdrop-filter: blur(5px);
}

.account-group {
  display: flex;
  align-items: flex-start; /* Align top for better multiline handling */
  padding: 0.5rem 0;
  text-align: left;
}

.side-label {
  background-color: #f0f0f0;
  color: #555;
  font-size: 0.8rem;
  padding: 0.2rem 0.6rem;
  border-radius: 4px;
  margin-right: 1rem;
  white-space: nowrap;
  margin-top: 0.1rem; /* Visual alignment */
}

.account-info {
  display: flex;
  flex-direction: column;
}

.account-info .name {
  font-weight: bold;
  font-size: 1rem;
  color: #333;
  margin-bottom: 0.2rem;
}

.bank-row {
  display: flex;
  align-items: flex-end; /* Align icon with bottom line of text */
  gap: 0.5rem;
}

.account-info .bank {
  font-size: 0.9rem;
  color: #666;
  letter-spacing: 0.5px;
}

.divider {
  height: 1px;
  background-color: #eee;
  margin: 1rem 0;
}


.info-item {
  margin-bottom: 1.5rem;
}

.info-item:last-child {
  margin-bottom: 0;
}

.label {
  display: block;
  font-size: 0.9rem;
  color: var(--pastel-pink-dark);
  margin-bottom: 0.5rem;
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.value {
  font-size: 1.2rem;
  color: #333;
}

.footer {
  padding: 2rem;
  text-align: center;
  font-size: 0.9rem;
  color: var(--text-light);
  background-color: var(--pastel-cream);
}

/* Responsive adjustments */
@media (max-width: 480px) {
  .mobile-container {
    box-shadow: none;
  }
}

.copy-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin-top: 0.5rem;
  padding: 0.4rem;
  color: var(--pastel-pink-dark);
  background-color: transparent;
  border: 1px solid var(--pastel-pink-dark);
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s;
}

.copy-btn:hover {
  background-color: var(--pastel-pink);
  color: #fff;
  border-color: var(--pastel-pink);
}

.copy-btn-sm {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin-top: 0;
  padding: 0.2rem 0.4rem;
  color: var(--text-light);
  background-color: var(--pastel-cream);
  border: 1px solid #FFE4E1; /* Mists Rose border */
  border-radius: 4px;
  cursor: pointer;
  align-self: flex-end; /* Align button to bottom of row */
  margin-bottom: 3px; /* visual adjustment to align with text baseline */
  transition: all 0.2s;
}

.copy-btn-sm:hover {
  background-color: var(--pastel-pink);
  color: #fff;
  border-color: var(--pastel-pink);
}
</style>
