<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import BlossomEffect from './components/BlossomEffect.vue'; // Import effect

// Helper to resolve paths relative to base URL (for deployment)
const resolvePath = (path) => {
  return new URL(path, import.meta.url).href;
}

// Or more simply, since these are in public folder, just prepend BASE_URL
const basePath = import.meta.env.BASE_URL;

const images = [
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-45-34.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-46-02.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-48-31.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-49-03.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-49-10.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-49-16.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-49-49 001.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-50-23 003.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-51-01 005.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-51-15 007.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-51-54 009.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-52-13 011.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-52-26 012.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-52-53 013.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-56-26 001.jpeg`,
  `${basePath}imgs/KakaoTalk_Photo_2026-02-07-18-56-29 002.jpeg`
];

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

const copyToClipboard = (text) => {
  navigator.clipboard.writeText(text).then(() => {
    alert('복사되었습니다: ' + text);
  }).catch(err => {
    console.error('복사 실패:', err);
    alert('복사하기가 지원되지 않는 환경입니다.');
  });
};
</script>

<template>
  <div class="mobile-container">
    <BlossomEffect />
    <header class="header">
      <h1 class="title">Wedding Invitation</h1>
      <p class="subtitle">We invite <br>
        you to our special day</p>
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
        <h2>소중한 분들을 초대합니다</h2>
        <p class="message">
          두 사람이 하나가 되는 뜻깊은 날,<br>
          귀한 걸음 하시어 축복해 주시면<br>
          더없는 기쁨으로 간직하겠습니다.
        </p>
      </section>


      <section class="details">
        <div class="info-item">
          <span class="label">Date</span>
          <span class="value">2026. 03. 28. SAT &nbsp; 05:40 PM</span>
        </div>
        <div class="info-item">
          <span class="label">Location</span>
          <span class="value">대전광역시 유성구 <br>테크노중앙로 161<br>스카이파크호텔 1층<br> 루이비스컨벤션</span>
          <button class="copy-btn" @click="copyToClipboard('대전광역시 유성구 테크노중앙로 161')" aria-label="주소 복사">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" viewBox="0 0 16 16">
              <path d="M4 1.5H3a2 2 0 0 0-2 2V14a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V3.5a2 2 0 0 0-2-2h-1v1h1a1 1 0 0 1 1 1V14a1 1 0 0 1-1 1H3a1 1 0 0 1-1-1V3.5a1 1 0 0 1 1-1h1v-1z"/>
              <path d="M9.5 1a.5.5 0 0 1 .5.5v1a.5.5 0 0 1-.5.5h-3a.5.5 0 0 1-.5-.5v-1a.5.5 0 0 1 .5-.5h3zm-3-1A1.5 1.5 0 0 0 5 1.5v1A1.5 1.5 0 0 0 6.5 4h3A1.5 1.5 0 0 0 11 2.5v-1A1.5 1.5 0 0 0 9.5 0h-3z"/>
            </svg>
          </button>
        </div>
      </section>


      <section class="account-section">
        <h3>마음 전하실 곳</h3>
        <p class="account-message">
          참석하지 못하시는 분들을 위해<br>
          계좌번호를 안내해 드립니다.<br>
          너른 양해 부탁드립니다.
        </p>
        
        <div class="account-box">
          <div class="account-group">
            <span class="side-label">신랑측</span>
            <div class="account-info">
              <span class="name">조준희</span>
              <div class="bank-row">
                <span class="bank">국민은행 <br>451-21-0312-080</span>
                <button class="copy-btn-sm" @click="copyToClipboard('451-21-0312-080')" aria-label="계좌번호 복사">
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
            <span class="side-label">신부측</span>
            <div class="account-info">
              <span class="name">신인숙</span>
              <div class="bank-row">
                <span class="bank">우리은행 <br>388-021774-12-001</span>
                <button class="copy-btn-sm" @click="copyToClipboard('388-021774-12-001')" aria-label="계좌번호 복사">
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
      <p>Thank you</p>
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
  margin-bottom: 3rem;
  background-color: #fff;
}

.slider-container {
  position: relative;
  width: 100%;
  overflow: hidden; /* Hide overflow images */
  aspect-ratio: 3/4; /* Adjust aspect ratio as needed */
  background-color: #f0f0f0;
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
  margin-bottom: 3rem;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.video-section video {
  width: 100%;
  display: block;
}


.details {
  background-color: #fff;
  padding: 2rem;
  border-radius: 8px;
  text-align: center;
  margin-bottom: 3rem;
}

.account-section {
  text-align: center;
  margin-bottom: 3rem;
  padding: 0 1rem;
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
  background-color: rgba(255, 255, 255, 0.6); /* Semi-transparent white */
  border: 1px solid var(--pastel-pink);
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 4px 15px rgba(255, 183, 197, 0.15); /* Soft pink shadow */
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
