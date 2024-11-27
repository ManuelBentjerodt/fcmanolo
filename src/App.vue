<!-- App.vue -->
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
  </div>
</template>


<script setup>
import confetti from 'canvas-confetti';
import { onMounted, ref } from 'vue';

const partyHorn = ref(null);

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
});
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
</style>
