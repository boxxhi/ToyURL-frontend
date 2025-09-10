<template>
  <div class="container">
    <div class="content">
      <p v-if="countdown > 0" class="countdown">{{ countdown }}</p>
      <p class="message">{{ message }}</p>
      <button
        @click="redirect"
        :disabled="isButtonDisabled"
        class="continue-button"
      >
        Continuar
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

// Reactive state
const message = ref('Por favor espera...');
const isButtonDisabled = ref(true);
const redirectUrl = ref(null);
const countdown = ref(3); // Countdown starts at 3 seconds

// Timer logic
let timer = null;
const startCountdown = () => {
  timer = setInterval(() => {
    countdown.value--;
    if (countdown.value <= 0) {
      clearInterval(timer);
      isButtonDisabled.value = false;
      message.value = 'Listo para redirigir.';
    }
  }, 1000);
};

// Cleanup timer on component unmount
onUnmounted(() => {
  if (timer) clearInterval(timer);
});

// Extract code and fetch redirect URL on mount
onMounted(async () => {
  // Extract code from URL path (e.g., /c/<code>)
  const path = window.location.pathname;
  const codeMatch = path.match(/\/c\/(.+)/);
  const code = codeMatch ? codeMatch[1] : null;

  if (!code) {
    message.value = 'URL inválida';
    return;
  }

  try {
    // Perform POST request to /lookup
    const response = await fetch('/api/lookup', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ code: code }),
    });
    const data = await response.json();

    if (data.redirectUrl) {
      redirectUrl.value = data.redirectUrl;
      // Start countdown
      startCountdown();
    } else {
      message.value = 'URL no encontrada';
    }
  } catch (error) {
    message.value = 'Error al conectar con el servidor';
    console.error('Error:', error);
  }
});

// Redirect function
const redirect = () => {
  if (redirectUrl.value) {
    window.location.href = redirectUrl.value;
  }
};
</script>

<style scoped>
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background-color: #fff;
}

.content {
  text-align: center;
}

.message {
  margin-bottom: 1rem;
  font-size: 1.155rem;

  color: var(--primary-color);
}

.countdown {
  margin-bottom: 1rem;
  font-size: 1rem;
  color: var(--primary-color);

  border-radius: 100%;
  border: 2px solid var(--muted-color);

  height: 300px;
  width: 300px;

  text-align: center;

  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 3rem;
}

.continue-button {
  padding: 0.5rem 1rem;
  background-color: var(--btn-color);
  color: var(--primary-color);
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
  font-size: 1rem;
}

.continue-button:disabled {
  cursor: not-allowed;
}

.continue-button:hover:not(:disabled) {
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 6px;
}
</style>

<style>

body {
  background-image: none;
  min-height: 100vh;
}

</style>
