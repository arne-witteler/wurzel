<template>
  <div v-if="isVisible" class="pyro">
    <div class="before"></div>
    <div class="after"></div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';

const isVisible = ref(true);

onMounted(() => {
  setTimeout(() => {
    isVisible.value = false;
  }, 3000);
});
</script>

<style scoped>
.pyro {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  pointer-events: none;
  z-index: 999;
  overflow: hidden;
}

.pyro>.before,
.pyro>.after {
  position: absolute;
  width: 6px;
  height: 6px;
  border-radius: 50%;
  box-shadow: 0 0 #fff;
  animation: bang 1.5s ease-out infinite backwards,
    gravity 1.5s ease-in infinite backwards,
    position 6s linear infinite backwards;
}

.pyro>.after {
  animation-delay: 1.25s, 1.25s, 1.25s;
  animation-duration: 1.25s, 1.25s, 6.25s;
}

@keyframes bang {
  to {
    box-shadow:
      -120px -210px #ff0055, 140px -320px #00ffaa, -40px -160px #00aaff, 190px -260px #ffcc00,
      -220px -120px #ff00ff, 80px -80px #00ffff, -180px -280px #ffff00, 240px -190px #ff5500,
      -90px -340px #aaff00, 120px -140px #aa00ff, -30px -290px #00ff00, 170px -90px #ff00aa,
      -150px -50px #3300ff, 50px -230px #ffea00, -250px -220px #00ff66, 210px -310px #ff0033;
  }
}

@keyframes gravity {
  to {
    transform: translateY(300px);
    opacity: 0;
  }
}

@keyframes position {

  0%,
  19.9% {
    margin-top: 15%;
    margin-left: 45%;
  }

  20%,
  39.9% {
    margin-top: 35%;
    margin-left: 25%;
  }

  40%,
  59.9% {
    margin-top: 20%;
    margin-left: 75%;
  }

  60%,
  79.9% {
    margin-top: 40%;
    margin-left: 15%;
  }

  80%,
  99.9% {
    margin-top: 25%;
    margin-left: 65%;
  }
}
</style>