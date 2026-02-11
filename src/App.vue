<script setup>
import { ref, computed, inject } from 'vue';

const nama = inject('nama');

const isPressed = ref(false);
const counter = ref(0);
const bubbles = ref([]);

const message = ref([
  "Gak mau",
  "Yakin nih?",
  "Kalau aku minta baik-baik gimana?",
  "Plis dong cantik",
  "Aku beliin coklat deh",
  "Atau seblak kesukaanmu?",
  `AYOLAH ${nama ? nama.toUpperCase() : 'SAYANG'}`,
  "Tapi :*(",
  "Aku bakal sakit nih",
  "Dah lah meninggoy",
  "Oke kamu ngomong sama hantuku",
  "Plis sayanggg",
  ":((((",
  "Plisss bangetttt",
  "Meninggoy beneran",
  "Jahat :("
]);

const spawnBubbles = () => {
  setInterval(() => {

    const id = Date.now();

    const leftPos = Math.random() * 100;
    const sizeScale = Math.random() * 2 + 1;
    const animDuration = Math.random() * 2 + 4;
    const slightWobble = Math.random() * 20 - 10;

    const newBubble = {
      id: id,
      style: {
        left: `${leftPos}%`,
        fontSize: `${sizeScale}rem`,
        animationDuration: `${animDuration}s`,
        '--wobble': `${slightWobble}px`
      }
    };

    bubbles.value.push(newBubble);

    setTimeout(() => {
      removeBubble(id);
    }, animDuration * 1000);

  }, 500);
};

const removeBubble = (id) => {
  bubbles.value = bubbles.value.filter(b => b.id !== id);
};

const changeMessage = computed(() => {
  return message.value[Math.min(counter.value, message.value.length - 1)];
})

const yesButtonSize = computed(() => {
  return (counter.value * 5 + 15) * 0.1 + 'rem';
})

const radiusSize = computed(() => {
  return (counter.value * 1.5) + 'px';
});

const increment = computed(() => {
  counter.value++;
})

const accept = () => {
  isPressed.value = true;
  spawnBubbles();
}

</script>

<template>
  <div v-if="isPressed" class="bubble-container">
    <span v-for="bubble in bubbles" :key="bubble.id" class="love-bubble" :style="bubble.style">
      ❤️
    </span>
  </div>
  <main class="container">


    <div class="content-wrapper">

      <template v-if="isPressed">
        <img src="https://media.tenor.com/gUiu1zyxfzYAAAAi/bear-kiss-bear-kisses.gif" alt="Bear Kiss"
          class="gif-image" />
        <h1 class="success-text">Yeyyyy! Akhirnya diterima juga! <br> Love you {{ nama }}-kuu!</h1>
      </template>

      <template v-else>
        <img
          :src="counter > 5 ? 'https://media.tenor.com/kTIu00Dc0WwAAAAi/hi.gif' : 'https://media.tenor.com/K0Op-0SpsvkAAAAj/dudu-cute.gif'"
          alt="Cute Bear" class="gif-image" />
        <h1 class="question-text">Mau gak jadi Valentine-ku?</h1>
        <div class="button-group">
          <button class="btn-confirm" :style="{
            backgroundColor: 'var(--color-background-btn)',
            color: 'white',
            borderRadius: counter < 5 ? '12px' : radiusSize,
            fontSize: counter === 0 ? '1.2rem' : yesButtonSize
          }" @click="accept">
            Iyaaaa
          </button>

          <button v-on:click="increment" class="btn-reject">
            {{ counter === 0 ? 'Gak mau' : changeMessage }}
          </button>
        </div>
      </template>

    </div>
  </main>
</template>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
  text-align: center;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  position: relative;
}

.content-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  max-width: 850px;
  width: 100%;
  z-index: 10;
}

.bubble-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 999;
  overflow: hidden;
}

.love-bubble {
  position: absolute;

  animation: floatUp ease-in forwards, sideWays ease-in-out infinite alternate;
}

@keyframes floatUp {
  0% {
    bottom: -10%;
    transform: scale(0.5);
  }


  100% {
    bottom: 110%;
    transform: scale(1);
  }
}

@keyframes sideWays {
  0% {
    margin-left: 0px;
  }

  100% {
    margin-left: var(--wobble, 20px);
  }
}

.gif-image {
  max-width: 100%;
  height: auto;
  border-radius: 15px;
  margin-bottom: 20px;
}

.success-text {
  font-size: 2rem;
}

.btn-reject {
  color: black;
}

button {
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 1.2rem;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 12px;
  border: black solid 1px;
  transition: all 0.3s ease;
}

@keyframes popIn {
  0% {
    transform: scale(0);
    opacity: 0;
  }

  100% {
    transform: scale(1);
    opacity: 1;
  }
}
</style>
