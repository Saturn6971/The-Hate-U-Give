<template>
  <v-main>
  <div>
    <div class="scroll-progress"></div>
    
    <!-- Hero Section - Nur das Buchcover -->
    <v-container fluid class="hero-section pa-0 ma-0" style="height: 100vh;">
      <v-row no-gutters class="fill-height" align="center" justify="center">
        <v-col cols="auto">
          <div class="hero-book-cover">
            <div class="book-spine-accent"></div>
            
            <div class="author-name">ANGIE THOMAS</div>
            
            <div class="character-illustration">
              <div class="character-hair"></div>
              <div class="character-headband"></div>
              <div class="character-face"></div>
            </div>
            
            <div class="title-icons">
              <span class="fist-icon">✊</span>
              <span class="coffee-icon">☕</span>
            </div>
            
            <div class="book-title">
              THE<br>
              HATE<br>
              U<br>
              GIVE
            </div>
            
            <div class="legs-illustration">
              <div class="shorts"></div>
              <div class="leg leg-left"></div>
              <div class="leg leg-right"></div>
              <div class="shoe shoe-left"></div>
              <div class="shoe shoe-right"></div>
            </div>
            
            <div class="award-badge">
              Deutscher<br>
              Jugendliteratur<br>
              Preis
            </div>
            
            <div class="quote-text">
              »Umwerfend und brillant, ein Klassiker!«<br>
              Bestsellerautor John Green
            </div>
            
            <div class="publisher-logo">cbj</div>
          </div>
          
          <div class="scroll-indicator" @click="scrollToMain">
            <div class="scroll-text">Scroll für mehr</div>
            <div class="scroll-arrow">↓</div>
          </div>
        </v-col>
      </v-row>
    </v-container>

    <!-- Main Interface Section -->
    <v-container fluid class="main-section pa-0 ma-0" style="min-height: 100vh;">
      <v-row no-gutters class="fill-height" align="center" justify="center">
        <v-col cols="12" class="text-center">
          <div class="book-interface" id="mainInterface" ref="mainInterface">
            <div class="book-cover-section">
              <div class="book-cover">
                <div class="book-spine-accent"></div>
                
                <div class="author-name">ANGIE THOMAS</div>
                
                <div class="character-illustration">
                  <div class="character-hair"></div>
                  <div class="character-headband"></div>
                  <div class="character-face"></div>
                </div>
                
                <div class="title-icons">
                  <span class="fist-icon">✊</span>
                  <span class="coffee-icon">☕</span>
                </div>
                
                <div class="book-title">
                  THE<br>
                  HATE<br>
                  U<br>
                  GIVE
                </div>
                
                <div class="legs-illustration">
                  <div class="shorts"></div>
                  <div class="leg leg-left"></div>
                  <div class="leg leg-right"></div>
                  <div class="shoe shoe-left"></div>
                  <div class="shoe shoe-right"></div>
                </div>
                
                <div class="award-badge">
                  Deutscher<br>
                  Jugendliteratur<br>
                  Preis
                </div>
                
                <div class="quote-text">
                  »Umwerfend und brillant, ein Klassiker!«<br>
                  Bestsellerautor John Green
  </div>

                <div class="publisher-logo">cbj</div>
              </div>
            </div>
            
            <div class="content-section">
              <div class="audio-player">
                <v-btn class="play-button" @click="togglePlay" fab small depressed>
                  {{ isPlaying ? '⏸' : '▶' }}
                </v-btn>
                <div class="waveform">
                  <div v-for="(bar, index) in 20" :key="index" 
                       class="frequency-bar"
                       :style="{ animationPlayState: isPlaying ? 'running' : 'paused' }"></div>
                </div>
              </div>
              
              <div class="content-placeholder">
                <div class="placeholder-title">BUCHINHALT</div>
                <div class="placeholder-subtitle">Kapitel, Zusammenfassung oder weitere Informationen</div>
                <div class="decorative-dots">
                  <div class="dot"></div>
                  <div class="dot"></div>
                  <div class="dot"></div>
                </div>
              </div>
            </div>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </div>
</v-main>
</template>

<script>
export default {
  name: 'BookInterface',
  data() {
    return {
      isPlaying: false
    }
  },
  mounted() {
    this.setupObservers();
    window.addEventListener('scroll', this.updateScrollProgress);
    window.addEventListener('scroll', this.handleParallax);
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.updateScrollProgress);
    window.removeEventListener('scroll', this.handleParallax);
  },
  methods: {
    scrollToMain() {
      this.$vuetify.goTo(this.$refs.mainInterface, {
        duration: 800,
        easing: 'easeInOutCubic',
        offset: 0
      });
    },
    togglePlay() {
      this.isPlaying = !this.isPlaying;
    },
    setupObservers() {
      const observerOptions = {
        threshold: 0.1,
        rootMargin: '0px 0px -100px 0px'
      };

      const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            entry.target.classList.add('revealed');
          }
        });
      }, observerOptions);

      observer.observe(this.$refs.mainInterface);

      const heroObserver = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          const heroSection = document.querySelector('.hero-section');
          if (entry.isIntersecting) {
            heroSection.classList.add('scrolled');
          } else {
            heroSection.classList.remove('scrolled');
          }
        });
      }, {
        threshold: 0.1,
        rootMargin: '0px 0px -50px 0px'
      });

      heroObserver.observe(this.$refs.mainInterface);
    },
    updateScrollProgress() {
      const scrollTop = window.pageYOffset;
      const docHeight = document.body.scrollHeight - window.innerHeight;
      const scrollPercent = (scrollTop / docHeight) * 100;
      
      document.querySelector('.scroll-progress').style.width = scrollPercent + '%';
    },
    handleParallax() {
      const scrolled = window.pageYOffset;
      const parallax = document.querySelector('.hero-section');
      const speed = scrolled * 0.5;
      
      if (parallax) {
        parallax.style.transform = `translateY(${speed}px)`;
      }
    }
  }
}
</script>


<style scoped>
/* All your original CSS styles go here exactly as they were */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

html {
    scroll-behavior: smooth;
}


v-main {
    font-family: 'Georgia', serif;
    background: linear-gradient(135deg, #000000 0%, #4a4a4a 50%, #ffffff 100%);
    background-attachment: fixed;
    overflow-x: hidden;
}

/* Hero Section - Nur das Buchcover */
.hero-section {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    perspective: 1000px;
    position: relative;
    transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
}

.hero-book-cover {
  scale: 1.4;
    width: 320px;
    height: 450px;
    background: #f8f8f8;
    border-radius: 12px;
    box-shadow: 
        0 25px 60px rgba(0,0,0,0.3),
        inset 0 1px 0 rgba(255,255,255,0.8);
    position: relative;
    transform: perspective(800px) rotateY(-10deg) rotateX(3deg);
    transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
    overflow: hidden;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    padding: 25px;
    text-align: center;
    animation: bookFloatIn 1.2s cubic-bezier(0.4, 0, 0.2, 1) forwards, 
               bookFloat 4s ease-in-out 1.5s infinite;
    opacity: 0;
}

@keyframes bookFloatIn {
    0% {
        opacity: 0;
        transform: perspective(800px) rotateY(-30deg) rotateX(10deg) translateY(100px) scale(0.8);
    }
    100% {
        opacity: 1;
        transform: perspective(800px) rotateY(-10deg) rotateX(3deg) translateY(0) scale(1);
    }
}

@keyframes bookFloat {
    0%, 100% {
        transform: perspective(800px) rotateY(-10deg) rotateX(3deg) translateY(0) scale(1);
    }
    25% {
        transform: perspective(800px) rotateY(-5deg) rotateX(1deg) translateY(-15px) scale(1.02);
    }
    50% {
        transform: perspective(800px) rotateY(-15deg) rotateX(5deg) translateY(-8px) scale(1.01);
    }
    75% {
        transform: perspective(800px) rotateY(-8deg) rotateX(2deg) translateY(-20px) scale(1.03);
    }
}

.hero-section.scrolled {
    opacity: 0;
    transform: translateY(-100px) scale(0.8);
    pointer-events: none;
}

.hero-book-cover:hover {
    transform: perspective(800px) rotateY(-5deg) rotateX(1deg) translateZ(30px) scale(1.05);
    box-shadow: 
        0 35px 80px rgba(0,0,0,0.4),
        inset 0 1px 0 rgba(255,255,255,0.8);
}

.scroll-indicator {
    position: absolute;
    bottom: 30px;
    left: 50%;
    transform: translateX(-50%);
    color: rgba(255,255,255,0.8);
    text-align: center;
    animation: fadeInUp 1.5s 1s both, bounce 2s 2s infinite;
    cursor: pointer;
}

.scroll-text {
    font-size: 16px;
    margin-bottom: 10px;
    letter-spacing: 1px;
    
}

.scroll-arrow {
    font-size: 24px;
    animation: bounce 2s infinite;
 
}

@keyframes bounce {
    0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
    40% { transform: translateY(-10px); }
    60% { transform: translateY(-5px); }
}

@keyframes fadeInUp {
    from {
        opacity: 0;
        transform: translateX(-50%) translateY(30px);
    }
    to {
        opacity: 1;
        transform: translateX(-50%) translateY(0);
    }
}

/* Main Interface Section */
.main-section {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 50px 20px;
    position: relative;
    scale: 1.2;
    transition: 1s;
    left: 25%;
    
    
   
}

.book-interface {
    background: rgba(255, 255, 255, 0.95);
    border-radius: 20px;
    padding: 30px;
    width: 90vw;
    max-width: 1000px;
    height: 70vh;
    max-height: 600px;
    box-shadow: 0 25px 50px rgba(0,0,0,0.2);
    display: flex;
    gap: 30px;
    backdrop-filter: blur(10px);
    border: 2px solid rgba(52, 152, 219, 0.3);
    opacity: 0;
    transform: translateY(80px) scale(0.9);
    transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
}

.book-interface.revealed {
    opacity: 1;
    transform: translateY(0) scale(1);
}

.book-cover-section {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    opacity: 0;
    transform: translateX(-50px);
    transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1) 0.2s;
}

.book-interface.revealed .book-cover-section {
    opacity: 1;
    transform: translateX(0);
}

.book-cover {
    width: 280px;
    height: 400px;
    background: #f8f8f8;
    border-radius: 12px;
    box-shadow: 
        0 15px 35px rgba(0,0,0,0.15),
        inset 0 1px 0 rgba(255,255,255,0.8);
    position: relative;
    transform: perspective(800px) rotateY(-15deg) rotateX(5deg);
    transition: all 0.4s ease;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    padding: 20px;
    text-align: center;
}

.book-cover:hover {
    transform: perspective(800px) rotateY(-10deg) rotateX(2deg) translateZ(20px);
    box-shadow: 
        0 25px 50px rgba(0,0,0,0.25),
        inset 0 1px 0 rgba(255,255,255,0.8);
}

.content-section {
    flex: 1.2;
    display: flex;
    flex-direction: column;
    gap: 20px;
    opacity: 0;
    transform: translateX(50px);
    transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1) 0.4s;
}

.book-interface.revealed .content-section {
    opacity: 1;
    transform: translateX(0);
}

.audio-player {
    background: linear-gradient(135deg, #ecf0f1 0%, #bdc3c7 100%);
    border-radius: 25px;
    padding: 15px 25px;
    display: flex;
    align-items: center;
    gap: 15px;
    box-shadow: 0 8px 20px rgba(0,0,0,0.1);
    transition: all 0.3s ease;
    opacity: 0;
    transform: translateY(30px);
    transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1) 0.6s;
    height: 100px;
}

.book-interface.revealed .audio-player {
    opacity: 1;
    transform: translateY(0);
}

.audio-player:hover {
    transform: translateY(-2px);
    box-shadow: 0 12px 25px rgba(0,0,0,0.15);
}

.content-placeholder {
    background: linear-gradient(135deg, #ecf0f1 0%, #d5dbdb 100%);
    border-radius: 15px;
    padding: 25px;
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    gap: 15px;
    box-shadow: 0 8px 20px rgba(0,0,0,0.08);
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
    opacity: 0;
    transform: translateY(40px);
    transition: all 0.7s cubic-bezier(0.4, 0, 0.2, 1) 0.8s;
}

.book-interface.revealed .content-placeholder {
    opacity: 1;
    transform: translateY(0);
}

.content-placeholder:hover {
    transform: translateY(-3px);
    box-shadow: 0 15px 30px rgba(0,0,0,0.12);
}

.content-placeholder::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
    animation: shimmer 2s infinite 1.5s;
}

@keyframes shimmer {
    0% { left: -100%; }
    100% { left: 100%; }
}

/* Alle ursprünglichen Styles für die Buchelemente */
.author-name {
    font-size: 16px;
    color: #B91C1C;
    font-weight: bold;
    letter-spacing: 3px;
    text-transform: uppercase;
    margin-bottom: 30px;
    font-family: 'Arial', sans-serif;
}





.book-title {
    font-size: 28px;
    font-weight: 900;
    color: #1a1a1a;
    line-height: 1.1;
    margin-bottom: 150px;
    font-family: 'Arial Black', sans-serif;
    text-transform: uppercase;
}

.hero-book-cover .book-title {
    font-size: 32px;
}

.title-icons {
    display: flex;
    justify-content: space-between;
    width: 100%;
    margin: 10px 0;
}

.fist-icon, .coffee-icon {
    font-size: 20px;
}



.shoe-left {
    left: 0;
}

.shoe-right {
    right: 0;
}

.shoe::after {
    content: '';
    position: absolute;
    bottom: -3px;
    left: 5px;
    width: 20px;
    height: 8px;
    background: #fff;
    border-radius: 10px;
}

.award-badge {
    position: absolute;
    bottom: 50px;
    left: 20px;
    width: 70px;
    height: 70px;
    background: linear-gradient(135deg, #DAA520 0%, #B8860B 100%);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 9px;
    color: white;
    font-weight: bold;
    text-align: center;
    box-shadow: 0 5px 15px rgba(218, 165, 32, 0.4);
    line-height: 1.1;
}

.quote-text {
    position: absolute;
    bottom: 20px;
    left: 20px;
    right: 20px;
    font-size: 10px;
    color: #666;
    text-align: center;
    font-style: italic;
}

.publisher-logo {
    position: absolute;
    bottom: 10px;
    right: 15px;
    background: #B91C1C;
    color: white;
    padding: 3px 8px;
    border-radius: 3px;
    font-size: 12px;
    font-weight: bold;
}

.play-button {
  height: 100px;
  background: linear-gradient(135deg, #3498db 0%, #2980b9 100%);
  color: white;
  box-shadow: 0 8px 24px rgba(41, 128, 185, 0.25);
  border: none;
}

.play-button:hover {
    transform: scale(1.1);
    background: linear-gradient(135deg, #3498db 0%, #2980b9 100%);
}

.waveform {
    flex: 1;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 2px;
    padding: 0 10px;
}

.frequency-bar {
    width: 3px;
    background: linear-gradient(180deg, #3498db 0%, #2980b9 100%);
    border-radius: 2px;
    animation: frequency 0.8s ease-in-out infinite;
}

.frequency-bar:nth-child(1) { height: 20px; animation-delay: 0s; }
.frequency-bar:nth-child(2) { height: 35px; animation-delay: 0.1s; }
.frequency-bar:nth-child(3) { height: 15px; animation-delay: 0.2s; }
.frequency-bar:nth-child(4) { height: 30px; animation-delay: 0.3s; }
.frequency-bar:nth-child(5) { height: 25px; animation-delay: 0.4s; }
.frequency-bar:nth-child(6) { height: 40px; animation-delay: 0.5s; }
.frequency-bar:nth-child(7) { height: 18px; animation-delay: 0.6s; }
.frequency-bar:nth-child(8) { height: 32px; animation-delay: 0.7s; }
.frequency-bar:nth-child(9) { height: 28px; animation-delay: 0.8s; }
.frequency-bar:nth-child(10) { height: 22px; animation-delay: 0.9s; }
.frequency-bar:nth-child(11) { height: 35px; animation-delay: 1s; }
.frequency-bar:nth-child(12) { height: 16px; animation-delay: 1.1s; }
.frequency-bar:nth-child(13) { height: 29px; animation-delay: 1.2s; }
.frequency-bar:nth-child(14) { height: 24px; animation-delay: 1.3s; }
.frequency-bar:nth-child(15) { height: 38px; animation-delay: 1.4s; }
.frequency-bar:nth-child(16) { height: 20px; animation-delay: 1.5s; }
.frequency-bar:nth-child(17) { height: 33px; animation-delay: 1.6s; }
.frequency-bar:nth-child(18) { height: 26px; animation-delay: 1.7s; }
.frequency-bar:nth-child(19) { height: 31px; animation-delay: 1.8s; }
.frequency-bar:nth-child(20) { height: 19px; animation-delay: 1.9s; }

@keyframes frequency {
    0%, 100% { 
        transform: scaleY(0.3);
        opacity: 0.6;
    }
    50% { 
        transform: scaleY(1);
        opacity: 1;
    }
}

.placeholder-title {
    font-size: 24px;
    color: #95a5a6;
    font-weight: 300;
    letter-spacing: 1px;
}

.placeholder-subtitle {
    font-size: 16px;
    color: #bdc3c7;
    font-weight: 300;
}

.decorative-dots {
    display: flex;
    gap: 8px;
    margin-top: 10px;
}

.dot {
    width: 8px;
    height: 8px;
    background: #bdc3c7;
    border-radius: 50%;
    animation: pulse 1.5s ease-in-out infinite;
}

.dot:nth-child(2) { animation-delay: 0.3s; }
.dot:nth-child(3) { animation-delay: 0.6s; }

@keyframes pulse {
    0%, 100% { opacity: 0.3; transform: scale(1); }
    50% { opacity: 1; transform: scale(1.2); }
}

.book-spine-accent {
    position: absolute;
    left: -10px;
    top: 0;
    width: 8px;
    height: 100%;
    background: linear-gradient(180deg, #B91C1C 0%, #991B1B 100%);
    border-radius: 4px;
    box-shadow: 0 0 15px rgba(185, 28, 28, 0.3);
}

/* Responsive Design */
@media (max-width: 768px) {
    .hero-book-cover {
        width: 280px;
        height: 380px;
        padding: 20px;
    }

    .book-interface {
        flex-direction: column;
        height: auto;
        gap: 20px;
        padding: 20px;
    }
    
    .book-cover {
        width: 200px;
        height: 280px;
        transform: none;
    }
    
    .book-cover:hover {
        transform: scale(1.05);
    }
    
    .book-title {
        font-size: 20px;
    }
    
    .content-section {
        gap: 15px;
    }

    .scroll-text {
        font-size: 14px;
    }
}

/* Scroll Progress Indicator */
.scroll-progress {
    position: fixed;
    top: 0;
    left: 0;
    width: 0%;
    height: 3px;
    background: linear-gradient(90deg, #B91C1C, #3498db);
    z-index: 1000;
    transition: width 0.1s ease;
}

html, body {
  min-height: 100vh;
  height: 100%;
  margin: 0;
  padding: 0;
  background: linear-gradient(135deg, #000000 0%, #4a4a4a 50%, #ffffff 100%);
  background-attachment: fixed;
}
</style>