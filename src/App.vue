<template>
  <div class="app-container">
    <h1 class="title">{{ title }}</h1>
    <div class="grid-container">
      <div 
        v-for="(card, index) in cards" 
        :key="index"
        class="glass-card"
        @click="handleCardClick(card)"
      >
        <div class="card-icon">
          <img :src="card.image" :alt="card.title" />
        </div>
        <h3 class="card-title">{{ card.title }}</h3>
        <p class="card-text">{{ card.description }}</p>
      </div>
    </div>
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
          image: 'src/res/thethinker.png',
          title: 'Who am I?',
          description: 'to be done'
        },
        {
          image: 'src/res/mc-cat.png',
          title: 'My Minecraft Server',
          description: 'Running Folia, 1.21.4-1.21.8 & Bedrock(only latest version), see web map'
        },
        {
          image: '...',
          title: '...',
          description: '...'
        },
        {
          image: '...',
          title: '...',
          description: '...'
        }
      ]
    }
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
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
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
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 16px;
  max-width: 600px;
  width: 100%;
  padding: 0 20px;
}

.glass-card {
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  padding: 24px;
  min-height: 160px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
  cursor: pointer;
}

.glass-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
}

.glass-card:hover {
  transform: translateY(-4px);
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 255, 255, 0.3);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.card-icon {
  width: 40px;
  height: 40px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 12px;
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(255, 255, 255, 0.1);
  overflow: hidden;
}

.card-icon img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 8px;
}

.card-title {
  color: white;
  font-size: 1rem;
  font-weight: 500;
  margin-bottom: 8px;
  text-align: center;
}

.card-text {
  color: rgba(255, 255, 255, 0.8);
  font-size: 0.875rem;
  text-align: center;
  line-height: 1.4;
}

@media (max-width: 768px) {
  .grid-container {
    grid-template-columns: 1fr;
    gap: 12px;
    max-width: 400px;
  }
  
  .title {
    font-size: 2rem;
  }
  
  .glass-card {
    min-height: 140px;
    padding: 20px;
  }
}

@media (max-width: 480px) {
  .grid-container {
    grid-template-columns: 1fr;
    gap: 12px;
  }
}
</style>