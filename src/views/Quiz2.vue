// CardStack.vue component
<template>
  <div class="container">
    <h2 class="title">Tap the arrow in the card to swipe the card towards the right direction</h2>
    <div class="flex">
      <p>← Pyhsical change</p>
      <p>Chemical change →</p>
    </div>
    <div class="card-stack">
      <a href="#" class="buttons prev" @click.prevent="handlePrevClick">←
      </a>
      <a href="#" class="buttons next" @click.prevent="handleNextClick">→</a>

      <ul class="card-list">
        <li v-for="(card, index) in visibleCards" :key="card.id"
          :class="['card', { 'activeNow': index === 0, 'transformThis': card.animateOut, 'transformPrev': card.animatePrev }]"
          :style="{ backgroundColor: card.bgColor }">
          {{ card.title }}
        </li>
      </ul>
    </div>

    <!-- Custom Toast -->
    <div class="custom-toast"
      :class="{ 'show': showToast, 'error': toastType === 'error', 'success': toastType === 'success' }">
      {{ toastMessage }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cards: [
        { id: 1, title: 'Rusting of Iron', type: 'chemical', bgColor: '#3498db' },
        { id: 2, title: 'Cutting Paper', type: 'physical', bgColor: '#e74c3c' },
        { id: 3, title: 'Burning Wood', type: 'chemical', bgColor: '#f39c12' },
        { id: 4, title: 'Folding Clothes', type: 'physical', bgColor: '#2ecc71' },
        { id: 5, title: 'Melting Ice', type: 'physical', bgColor: '#9b59b6' },
        { id: 6, title: 'Cooking Food', type: 'chemical', bgColor: '#1abc9c' },
        { id: 7, title: 'Breaking Glass', type: 'physical', bgColor: '#e67e22' },
      ],
      visibleCards: [],
      showToast: false,
      toastMessage: '',
      toastType: 'success',
      toastTimeout: null
    };
  },

  created() {
    // Initialize visible cards
    this.visibleCards = this.cards.slice(0, 5).map(card => ({
      ...card,
      animateOut: false,
      animatePrev: false
    }));
  },

  methods: {
    handlePrevClick() {
      const currentCard = this.visibleCards[0];

      if (currentCard.type === 'physical') {
        // This is correct for physical change
        this.showSuccessToast('Correct! Physical changes can be reversed.');
        this.removeCurrentCard();
      } else {
        // This is incorrect for chemical change
        this.showErrorToast('Incorrect! Chemical changes need the right arrow.');
      }
    },

    handleNextClick() {
      const currentCard = this.visibleCards[0];

      if (currentCard.type === 'chemical') {
        // This is correct for chemical change
        this.showSuccessToast('Correct! Chemical changes create new substances.');
        this.removeCurrentCard();
      } else {
        // This is incorrect for physical change
        this.showErrorToast('Incorrect! Physical changes need the left arrow.');
      }
    },

    removeCurrentCard() {
      // Animate the card out
      this.visibleCards[0].animateOut = true;

      // After animation completes, remove the card and add a new one
      setTimeout(() => {
        // Remove first card
        this.visibleCards.shift();

        // Find the next card that isn't in visibleCards
        const usedIds = this.visibleCards.map(card => card.id);
        const availableCards = this.cards.filter(card => !usedIds.includes(card.id));

        if (availableCards.length > 0) {
          // Add a new card to the end
          this.visibleCards.push({
            ...availableCards[0],
            animateOut: false,
            animatePrev: false
          });
        }
      }, 500);
    },

    showSuccessToast(message) {
      this.toastMessage = message;
      this.toastType = 'success';
      this.showToast = true;

      this.clearToastTimeout();
      this.toastTimeout = setTimeout(() => {
        this.showToast = false;
      }, 3000);
    },

    showErrorToast(message) {
      this.toastMessage = message;
      this.toastType = 'error';
      this.showToast = true;

      this.clearToastTimeout();
      this.toastTimeout = setTimeout(() => {
        this.showToast = false;
      }, 3000);
    },

    clearToastTimeout() {
      if (this.toastTimeout) {
        clearTimeout(this.toastTimeout);
      }
    }
  }
};
</script>

<style lang="less">
@default-diff: 12px;
@default-width: 100%;
@default-min: 10%;

.container {
  width: 100%;
  height: 900px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #fff;
  padding: 50px 2rem;
  position: relative;

  h2 {
    text-align: center;
    margin-bottom: 1rem;
  }

  .flex {
    display: flex;
    flex-direction: row;
    width: 500px;
    align-items: center;
    justify-content: space-between;

    p {
      margin: 10px 0;
    }
  }

  .card-stack {
    width: 100%;
    max-width: 500px;
    // height: 250px;
    position: relative;
    margin: 20px auto;
    display: flex;
    justify-content: center;
    align-items: center;

    .buttons {
      position: absolute;
      background: skyblue;

      border-radius: 5px;
      width: auto;
      height: auto;
      padding: 1rem;
      top: 50%;
      color: blue;
      text-align: center;
      line-height: 0;
      text-decoration: none;
      font-size: 24px;
      z-index: 100;
      outline: none;
      transition: all 0.2s ease;
    }


    .prev {
      left: 0px;
    }

    .next {
      right: 0px;
    }

    .card-list {
      width: 100%;
      padding: 0;
      margin: 0;
      position: relative;

      li {
        transition: all 100ms ease-in-out;
        border-radius: 8px;
        position: absolute;
        list-style: none;
        left: 0;
        right: 0;
        margin: 0 auto;
        padding-top: 20px;
        text-align: center;
        background-size: cover;
        color: white;
        font-weight: bold;
        font-size: 20px;
        text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.8);
        display: flex;
        align-items: center;
        justify-content: center;

        &:nth-child(1) {
          top: 100px;
          height: 300px;
          width: 80%;
          z-index: 5;
        }

        &:nth-child(2) {
          top: 90px;
          height: 270px;
          width: 72%;
          z-index: 4;
        }

        &:nth-child(3) {
          top: 80px;
          height: 240px;
          width: 64%;
          z-index: 3;
        }

        &:nth-child(4) {
          top: 70px;
          height: 210px;
          width: 56%;
          z-index: 2;
        }

        &:nth-child(5) {
          top: 60px;
          height: 180px;
          width: 48%;
          z-index: 1;
        }
      }
    }

    &:hover {
      >.buttons.prev {
        display: block;
        animation: bounceInLeft 200ms;
      }

      >.buttons.next {
        display: block;
        animation: bounceInRight 200ms;
      }
    }
  }

  /* Media Queries */
  @media (max-width: 767px) {
    .container {
      padding: 1rem;
    }

    .card-stack {
      width: 90%;
      max-width: 400px;
      height: 200px;
    }

    .flex {
      width: 100%;
      gap: 10px;
      flex-direction: row;
    }

    .card-list li {
      height: 200px;
      width: 100%;
      top: 50px;
    }

    .buttons {
      font-size: 18px;
      width: 30px;
      height: 30px;
      top: 45%;
    }
  }

  .transformThis {
    animation: scaleDown 500ms;
  }

  .transformPrev {
    animation: scaleUp 100ms;
    display: none;
  }

  /* Custom Toast Styles */
  .custom-toast {
    position: fixed;
    top: 30px;
    right: 0%;
    transform: translateX(-10%);
    padding: 12px 24px;
    border-radius: 4px;
    color: white;
    font-weight: 500;
    opacity: 0;
    transition: opacity 0.3s ease;
    z-index: 1000;

    &.show {
      opacity: 1;
    }

    &.success {
      background-color: #4CAF50;
    }

    &.error {
      background-color: #F44336;
    }
  }

  /* Animation Keyframes */
  @keyframes scaleUp {
    0% {
      transform: scale(1.2) translateY(50px);
      opacity: 0;
    }

    100% {
      transform: scale(1) translateY(0);
      opacity: 1;
    }
  }

  @keyframes scaleDown {
    0% {
      transform: scale(1) translateY(0);
      opacity: 1;
    }

    100% {
      transform: scale(1.2) translateY(100px);
      opacity: 0;
    }
  }

  @keyframes bounceInLeft {
    0% {
      opacity: 0;
      transform: translateX(40px);
    }

    100% {
      transform: translateX(0);
    }
  }

  @keyframes bounceInRight {
    0% {
      opacity: 0;
      transform: translateX(-40px);
    }

    100% {
      transform: translateX(0);
    }
  }
}
</style>