<template>
  <div class="app-container">
    <h1 class="title">{{ title }}</h1>
    <div class="grid-container">
      <div 
        v-for="(card, index) in cards" 
        :key="index"
        class="glass-card"
        :class="{ 'card-hidden': expandedCardIndex === index }"
        :ref="el => cardRefs[index] = el"
        @click="handleCardClick(card, index, $event)"
      >
        <img :src="card.image" :alt="card.title" class="card-bg-image" />
        <div class="card-content">
          <h3 class="card-title">{{ card.title }}</h3>
          <p class="card-text">{{ card.description }}</p>
        </div>
      </div>
    </div>

    <!-- Overlay backdrop and expanded card -->
    <Transition name="backdrop">
      <div 
        v-if="expandedCardIndex !== null"
        class="overlay-backdrop"
        @click="closeOverlay"
      ></div>
    </Transition>

    <Transition name="modal" @before-enter="beforeEnter" @after-leave="afterLeave">
      <div 
        v-if="expandedCardIndex !== null"
        class="expanded-card"
        :style="expandedStyle"
        @click.stop
      >
        <!-- Full background image like wallpaper -->
        <a v-if="cards[expandedCardIndex].link" :href="cards[expandedCardIndex].link" target="_blank" rel="noopener noreferrer" class="expanded-image-link">
          <img :src="cards[expandedCardIndex].image" :alt="cards[expandedCardIndex].title" class="expanded-bg-image" />
        </a>
        <img v-else :src="cards[expandedCardIndex].image" :alt="cards[expandedCardIndex].title" class="expanded-bg-image" />
        
        <button class="close-btn" @click="closeOverlay" aria-label="Close">
          <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
          </svg>
        </button>
        
        <div class="expanded-content">
          <h2 class="expanded-title">{{ cards[expandedCardIndex].title }}</h2>
          <p class="expanded-description">{{ cards[expandedCardIndex].description }}</p>
          <div class="expanded-details">
            <p>{{ cards[expandedCardIndex].details || 'More details coming soon...' }}</p>
          </div>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      title: 'Hello...',
      cards: [
        {
          image: '/res/thethinker.png',
          title: 'Who am I?',
          description: '',
          details: 'Someone in El Salvador. If you are here you probably know me already',
          link: ''
        },
        {
          image: '/res/mc-cat.png',
          title: '',
          description: '',
          details: '',
          link: 'https://crissuper20.com/map/'
        },
        {
          image: '',
          title: 'Soon™',
          description: '',
          details: '',
          link: ''
        },
        {
          image: '',
          title: 'Soon™',
          description: '',
          details: '',
          link: ''
        }
      ],
      expandedCardIndex: null,
      cardRefs: [],
      expandedStyle: {},
      clickedCardRect: null
    }
  },
  methods: {
    handleCardClick(card, index, event) {
      const cardEl = this.cardRefs[index];
      if (cardEl) {
        this.clickedCardRect = cardEl.getBoundingClientRect();
      }
      this.expandedCardIndex = index;
      document.body.style.overflow = 'hidden';
    },
    closeOverlay() {
      this.expandedCardIndex = null;
      document.body.style.overflow = '';
    },
    beforeEnter(el) {
      if (this.clickedCardRect) {
        const rect = this.clickedCardRect;
        el.style.transformOrigin = `${rect.left + rect.width / 2}px ${rect.top + rect.height / 2}px`;
      }
    },
    afterLeave() {
      this.clickedCardRect = null;
    }
  },
  mounted() {
    window.addEventListener('keydown', (e) => {
      if (e.key === 'Escape' && this.expandedCardIndex !== null) {
        this.closeOverlay();
      }
    });
  }
}
</script>

<style scoped>
.app-container {
  padding: 20px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: #000;
  background-attachment: fixed;
  position: relative;
}

.app-container::before {
  content: '';
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><defs><pattern id="grain" width="100" height="100" patternUnits="userSpaceOnUse"><circle cx="25" cy="25" r="1" fill="rgba(255,255,255,0.1)"/><circle cx="75" cy="75" r="1" fill="rgba(255,255,255,0.1)"/><circle cx="50" cy="10" r="0.5" fill="rgba(255,255,255,0.15)"/><circle cx="90" cy="40" r="0.8" fill="rgba(255,255,255,0.08)"/><circle cx="10" cy="80" r="1.2" fill="rgba(255,255,255,0.12)"/></pattern></defs><rect width="100" height="100" fill="url(%23grain)"/></svg>');
  opacity: 0.3;
  pointer-events: none;
  z-index: -1;
}

.title {
  color: white;
  font-size: 2.5rem;
  font-weight: 300;
  margin-bottom: 2rem;
  text-shadow: 0 2px 4px rgba(0,0,0,0.3);
  letter-spacing: -0.5px;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  max-width: 900px;
  width: 100%;
  padding: 0 20px;
}

.glass-card {
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  padding: 0;
  min-height: 250px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
  cursor: pointer;
}

.glass-card.card-hidden {
  opacity: 0;
  transform: scale(0.95);
}

.card-bg-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: 0;
  transition: transform 0.5s ease;
}

.glass-card:hover .card-bg-image {
  transform: scale(1.1);
}

.card-content {
  position: relative;
  z-index: 2;
  width: 100%;
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background: linear-gradient(to top, rgba(0,0,0,0.7) 0%, transparent 100%);
}

.glass-card::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,0.2);
  z-index: 1;
  transition: background 0.3s;
}

.glass-card:hover::after {
  background: rgba(0,0,0,0.1);
}

.glass-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
  z-index: 3;
}

.glass-card:hover {
  transform: translateY(-4px);
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 255, 255, 0.3);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
}

.card-title {
  color: white;
  font-size: 1.1rem;
  font-weight: 600;
  margin-bottom: 8px;
  text-align: center;
  text-shadow: 0 2px 4px rgba(0,0,0,0.5);
}

.card-text {
  color: rgba(255, 255, 255, 0.9);
  font-size: 0.875rem;
  text-align: center;
  line-height: 1.4;
  text-shadow: 0 1px 2px rgba(0,0,0,0.5);
}

.overlay-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  z-index: 99;
}

.expanded-card {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 90vw;
  max-width: 600px;
  height: 70vh;
  max-height: 500px;
  background: rgba(0, 0, 0, 0.3);
  border-radius: 24px;
  border: 1px solid rgba(255, 255, 255, 0.15);
  z-index: 100;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  box-shadow: 
    0 25px 50px -12px rgba(0, 0, 0, 0.5),
    0 0 0 1px rgba(255, 255, 255, 0.1);
}

.expanded-bg-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center center;
  z-index: 0;
}

.close-btn {
  position: absolute;
  top: 16px;
  right: 16px;
  width: 50px;
  height: 50px;
  background: rgba(0, 0, 0, 0.4);
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
  transition: all 0.2s ease;
  color: white;
}

.close-btn:hover {
  background: rgba(0, 0, 0, 0.6);
  transform: rotate(90deg);
}

.expanded-image-link {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  cursor: pointer;
}

.expanded-image-link .expanded-bg-image {
  transition: transform 0.3s ease, filter 0.3s ease;
}

.expanded-image-link:hover .expanded-bg-image {
  transform: scale(1.02);
  filter: brightness(1.1);
}

.expanded-content {
  position: relative;
  z-index: 2;
  padding: 32px;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.95) 0%, rgba(0, 0, 0, 0.8) 50%, transparent 100%);
}

.expanded-title {
  color: white;
  font-size: 1.75rem;
  font-weight: 600;
  margin-bottom: 12px;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
}

.expanded-description {
  color: rgba(255, 255, 255, 0.9);
  font-size: 1rem;
  line-height: 1.6;
  margin-bottom: 16px;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.5);
}

.expanded-details {
  padding-top: 16px;
  border-top: 1px solid rgba(255, 255, 255, 0.2);
  color: rgba(255, 255, 255, 0.85);
  font-size: 0.95rem;
  line-height: 1.6;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.5);
}

.backdrop-enter-active,
.backdrop-leave-active {
  transition: opacity 0.3s ease;
}

.backdrop-enter-from,
.backdrop-leave-to {
  opacity: 0;
}

.modal-enter-active {
  transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
}

.modal-leave-active {
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
}

.modal-enter-from {
  opacity: 0;
  transform: translate(-50%, -50%) scale(0.7);
}

.modal-leave-to {
  opacity: 0;
  transform: translate(-50%, -50%) scale(0.9);
}

@media (max-width: 768px) {
  .grid-container {
    grid-template-columns: 1fr;
    gap: 16px;
    max-width: 400px;
  }
  
  .title {
    font-size: 2rem;
  }
  
  .glass-card {
    min-height: 200px;
  }

  .expanded-card {
    width: 95vw;
    height: 80vh;
    max-height: 600px;
    border-radius: 16px;
  }

  .expanded-content {
    padding: 24px;
  }

  .expanded-title {
    font-size: 1.5rem;
  }
}

@media (max-width: 480px) {
  .grid-container {
    grid-template-columns: 1fr;
    gap: 12px;
  }
}
</style>