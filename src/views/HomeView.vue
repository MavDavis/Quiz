<template>
  <div class="quiz-container">
    <h2 class="title">Tap to Discover States of Matter!</h2>

    <div class="card-grid">
      <div v-for="(item, index) in cards" :key="index" class="card-wrapper" :class="{ flipped: item.flipped }"
        @click="handleClick(item)">
        <div class="card">
          <div class="card-front" :style="{ background: item.color }">
            <div class="icon">{{ item.icon }}</div>
            <p>{{ item.label }}</p>
          </div>
          <div class="card-back">
            <p class="definition">{{ item.definition }}</p>
          </div>
        </div>
      </div>
    </div>

    <router-link to="/quiz2" class="next-btn">
      Next →
    </router-link>

    <!-- Toast Container -->
    <div class="toast-container">
      <div v-for="toast in toasts" :key="toast.id" :class="['toast', toast.type]">
        {{ toast.message }}
      </div>
    </div>
  </div>
</template>

<script>
let idCounter = 0;

export default {
  name: 'StatesOfMatterQuiz',
  data() {
    return {
      toasts: [],
      cards: [
        {
          label: 'Steam',
          definition: 'Gas\nA gas expands to fill any container. Its particles move freely and spread apart.',
          color: 'linear-gradient(to bottom right, #38bdf8, #06b6d4)',
          flipped: false,
          isCorrect: true,
          icon: '💨',
        },
        {
          label: 'Water',
          definition: 'Liquid\nA liquid takes the shape of its container but keeps its volume.',
          color: 'linear-gradient(to bottom right, #a78bfa, #8b5cf6)',
          flipped: false,
          isCorrect: true,
          icon: '💧',
        },
        {
          label: 'Ice',
          definition: 'Solid\nFixed shape & volume. Particles vibrate but don’t move freely.',
          color: 'linear-gradient(to bottom right, #facc15, #f59e0b)',
          flipped: false,
          isCorrect: true,
          icon: '🧊',
        },
        {
          label: 'Lightning',
          definition: '',
          color: 'linear-gradient(to bottom right, #3b82f6, #06b6d4)',
          flipped: false,
          isCorrect: false,
          icon: '⚡',
        },
      ],
    };
  },
  methods: {
    handleClick(item) {
      if (item.isCorrect) {
        if (!item.flipped) {
          item.flipped = true;
          this.showToast(`${item.label} is a state of matter!`, 'success');
        }
      } else {
        this.showToast(`${item.label} is not a state of matter.`, 'error');
      }
    },
    showToast(message, type = 'success') {
      const id = idCounter++;
      this.toasts.push({ id, message, type });

      setTimeout(() => {
        this.toasts = this.toasts.filter((t) => t.id !== id);
      }, 3000);
    },
  },
};
</script>

<style scoped>
.quiz-container {
  padding: 2rem;
  max-width: 600px;
  margin: auto;
  text-align: center;
  position: relative;
}

.title {
  font-size: 1.5rem;
  margin-bottom: 2rem;
}

.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}


/* 
.card-wrapper {
  perspective: 1000px;
  cursor: pointer;
  position: relative;
} */

.card {
  width: 100%;
  height: 200px;
  max-width: none;
  /* remove this if you had max-width before */
  border-radius: 12px;
  transition: transform 0.6s;
  transform-style: preserve-3d;
  position: relative;
  box-shadow: 0 3px 10px rgb(0 0 0 / 0.2);

}

.card-wrapper.flipped .card {
  transform: rotateY(180deg);
}

.card-front,
.card-back {
  backface-visibility: hidden;
  border-radius: 12px;
  position: absolute;
  width: 100%;
  height: 100%;
  color: #fff;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 1rem;
}

.card-front {
  font-size: 1.1rem;
  font-weight: bold;
}

.card-back {
  background-color: #fff;
  color: #000;
  transform: rotateY(180deg);
  font-size: 0.85rem;
  white-space: pre-wrap;
}

.icon {
  font-size: 2rem;
  margin-bottom: 0.5rem;
}

.next-btn {
  margin-top: 2rem;
  text-decoration: none;
  background-color: #1d4ed8;
  color: white;
  border: none;
  padding: 0.8rem 2rem;
  border-radius: 10px;
  font-size: 1rem;
  cursor: pointer;
}

/* Toast Styles */
.toast-container {
  position: fixed;
  top: 1rem;
  right: 1rem;
  z-index: 9999;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.toast {
  padding: 0.8rem 1.2rem;
  border-radius: 8px;
  color: white;
  font-weight: 500;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  animation: fadeInOut 3s forwards;
}

.toast.success {
  background-color: #16a34a;
}

.toast.error {
  background-color: #dc2626;
}

@keyframes fadeInOut {
  0% {
    opacity: 0;
    transform: translateY(-10px);
  }

  10% {
    opacity: 1;
    transform: translateY(0);
  }

  90% {
    opacity: 1;
    transform: translateY(0);
  }

  100% {
    opacity: 0;
    transform: translateY(-10px);
  }
}
</style>
