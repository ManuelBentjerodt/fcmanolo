<template>
  <div class="birthday-container">
    <h1>¡Feliz Cumpleaños!</h1>
    <p>¡Qué tengas un día increíble!</p>
    <img 
      src="/src/images/mmj.jpg" 
      alt="Birthday Image" 
      class="birthday-image" 
      @click="handleClick" 
    />
    <audio ref="partyHorn" src="/audios/party-horn.mp3"></audio>

    <!-- Cronómetro -->
    <div class="countdown">
      <p>Tiempo estimado de entrega del regalo: </p>
      <div class="countdown-timer">
          <span>{{ days }} días </span>
          <span>{{ hours }} horas </span>
          <span>{{ minutes }} minutos </span>
          <span>{{ seconds }} segundos </span>
      </div>
    </div>
  </div>
</template>

<script setup>
import confetti from 'canvas-confetti';
import { onMounted, ref } from 'vue';

const partyHorn = ref(null);

// Cronómetro
const targetDate = new Date('2024-12-14T22:00:00');
const days = ref(0);
const hours = ref(0);
const minutes = ref(0);
const seconds = ref(0);

// Actualización del cronómetro
const updateCountdown = () => {
  const now = new Date();
  const diff = targetDate - now;

  if (diff > 0) {
    days.value = Math.floor(diff / (1000 * 60 * 60 * 24));
    hours.value = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    minutes.value = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
    seconds.value = Math.floor((diff % (1000 * 60)) / 1000);
  } else {
    days.value = hours.value = minutes.value = seconds.value = 0;
  }
};

onMounted(() => {
  // Lanzar confeti al cargar la página
  confetti({
    particleCount: 100,
    spread: 70,
    origin: { y: 0.6 }
  });

  // Reproducir sonido de corneta de fiesta al cargar la página
  if (partyHorn.value) {
    partyHorn.value.play().catch(error => {
      // console.error('Error playing audio:', error);
    });
  }

  // Actualizar cronómetro cada segundo
  setInterval(updateCountdown, 1000);

  // Inicializar el cronómetro
  updateCountdown();
});

// Función para manejar el clic en la imagen
const handleClick = () => {
  // Detener y reiniciar el audio
  if (partyHorn.value) {
    partyHorn.value.currentTime = 0; // Reinicia el tiempo de reproducción
    partyHorn.value.play().catch(error => {
      console.error('Error playing audio:', error);
    });
  }

  // Lanzar confeti al hacer clic en la imagen
  confetti({
    particleCount: 100,
    spread: 70,
    origin: { y: 0.6 }
  });
};
</script>

<style scoped>
/* Reset default margins and paddings */
body, html {
  margin: 0;
  padding: 0;
  height: 100%;
  overflow: hidden;
}

.birthday-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  background-color: #FFF0F5; /* Pastel Pink */
  text-align: center;
  font-family: 'Arial', sans-serif;
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  overflow: hidden;
}

h1 {
  color: #e97183; /* Light Pink */
  font-size: 3rem;
  margin-bottom: 20px;
  text-shadow: 2px 2px 4px rgba(255, 182, 193, 0.3);
}

p {
  color: #74aecf; /* Pale Blue */
  font-size: 1.5rem;
  font-weight: lighter;
  margin-bottom: 20px;
}

.birthday-image {
  max-width: 80%; /* Adjust the width as needed */
  max-height: 40vh; /* Limit height to 40% of viewport height */
  object-fit: contain; /* Maintain aspect ratio */
  border-radius: 10px; /* Optional: adds rounded corners */
  box-shadow: 0 4px 6px rgba(0,0,0,0.1); /* Optional: adds a subtle shadow */
  cursor: pointer; /* Cambio a mano cuando se pasa el ratón */
  transition: transform 0.1s ease-in-out; /* Animación para el efecto de botón */
}

/* Efecto de botón: reducir tamaño cuando se hace clic */
.birthday-image:active {
  transform: scale(0.95); /* Hace que la imagen se reduzca un poco al hacer clic */
}

.countdown {
  margin-top: 30px;
  font-size: 1.2rem;
}

.countdown-timer {
  font-size: 2rem;
  font-weight: bold;
  color: #e97183;
}


</style>
