<script setup>
import { ref, onMounted } from "vue";

const palagi = new Audio(
  new URL("../assets/audio/palagi.mp3", import.meta.url).href
);
palagi.volume = 0;

const sa_bawat_sandali = new Audio(
  new URL("../assets/audio/sa_bawat_sandali.mp3", import.meta.url).href
);
sa_bawat_sandali.volume = 0.5;

const open = ref(false);
const answer = ref(true);
const happy = ref(false);
const no_1 = ref(false);

const toggleOpen = () => {
  open.value = true;
  fadeInAudio(sa_bawat_sandali); // Fade in "Sa Bawat Sandali"
};

const toggleYes = () => {
  fadeOutAudio(sa_bawat_sandali); // Fade out "Sa Bawat Sandali"
  fadeInAudio(palagi); // Fade in "Palagi"
  // sa_bawat_sandali.pause();
  // palagi.play().catch((error) => console.error("Audio play failed:", error));
  no_1.value = false;
  answer.value = false;
  happy.value = true;
};

const toggleNo = () => {
  fadeOutAudio(palagi); // Fade out "Palagi"
  fadeInAudio(sa_bawat_sandali); // Fade in "Sa Bawat Sandali"
  answer.value = false;
  happy.value = false;
  no_1.value = true;
};

// Function to fade in audio smoothly
const fadeInAudio = (audio) => {
  audio.volume = 0;
  audio.play();
  let volume = 0;
  const interval = setInterval(() => {
    if (volume < 0.5) {
      volume += 0.05;
      audio.volume = volume;
    } else {
      clearInterval(interval);
    }
  }, 100);
};

// Function to fade out audio smoothly
const fadeOutAudio = (audio) => {
  let volume = audio.volume;
  const interval = setInterval(() => {
    if (volume > 0) {
      volume -= 0.05;
      audio.volume = Math.max(volume, 0);
    } else {
      audio.pause();
      clearInterval(interval);
    }
  }, 100);
};
</script>

<template>
  <img
    class="h-full w-full object-cover absolute inset-0"
    src="../assets/images/bg-valentine.jpg"
    alt="Valentine Background"
  />

  <main
    class="absolute inset-0 flex flex-col items-center justify-center text-red-800 border-3 border-red z-10 p-6"
  >
    <div v-if="open">
      <div class="flex flex-col items-center" v-if="answer">
        <p class="text-2xl text-center">
          Hello, My love 💖 <br />Will You be my Valentine?
          <span v-if="no_1">Please?</span>
        </p>

        <img
          v-if="answer"
          class="bg-cream border border-peach rounded-lg h-72 w-72 my-6"
          src="../assets/images/please-gif.gif"
          alt="please gif"
        />
      </div>

      <div class="flex flex-col items-center" v-if="no_1">
        <p class="text-2xl text-center">
          Awww 🥺🥺🥺 <br />Will You be my Valentine? Please?
        </p>

        <img
          v-if="no_1"
          class="bg-cream border border-peach rounded-lg h-72 w-72 my-6 p-6"
          src="../assets/images/please-2-gif.gif"
          alt="please gif"
        />
      </div>

      <div class="flex flex-col items-center" v-if="happy">
        <p class="text-2xl text-center">
          You said <span class="font-bold">YES</span>!!! See you on
          <span class="font-bold">Saturday</span>!
          <br />
          I Love You, my palagi 💖
        </p>

        <img
          class="bg-cream border border-peach rounded-lg h-72 w-72 my-6"
          src="../assets/images/hug-gif.gif"
          alt=""
        />
      </div>

      <div class="flex items-center justify-center gap-3">
        <button
          @click="toggleNo"
          class="btn-no"
          :class="no_1 ? 'btn-yes bg-red text-white' : ''"
        >
          no
        </button>
        <button
          @click="toggleYes"
          class="btn-yes"
          :class="happy ? 'btn-yes bg-red text-white' : ''"
        >
          YES
        </button>
      </div>
    </div>
    <div v-else><button @click="toggleOpen" class="btn-yes">Open</button></div>
  </main>
</template>
