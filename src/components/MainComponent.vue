<template>
  <main>
    <section class="main">
      <div class="banner-msg">
          <h1>¡Empieza a acortar enlaces gratis!</h1>
          <p>Usa nuestro acortador para mejorar el diseño de tus links</p>
        </div>

        <div class="container__form">
          <div class="shorter-form">
            <h2>Pega tu URL aqui</h2>
            <p>No requiere registro</p>
            <div class="shorter-form-input">
              <div class="ex">
                <input v-model="longUrl" type="url" placeholder="URL" required />
                <div v-if="error" class="error">{{ error }}</div>
              </div>

              <button @click="shortLink">Acortar</button>
            </div>
          </div>

          <div v-if="shortUrl" class="result">
            <p>
              Enlace acortado: <a :href="shortUrl" target="_blank">{{ shortUrl }}</a>
            </p>
            <button @click="copyToClipboard">Copiar</button>
          </div>
        </div>
    </section>

    <section class="info">
      <h1>Gestiona y personaliza tus enlaces</h1>
      <p>Registrate gratis y accede a tu panel de control donde podras gestionar y personalizar tus enlaces</p>
    </section>
  </main>
</template>

<script setup>
import { ref } from "vue";

const longUrl = ref("");
const shortUrl = ref("");

const error = ref("");
const showError = ref(false);

async function shortLink() {
  showError.value = true;

  if (longUrl.value === "") {
    error.value = "Por favor, ingresa un enlace válido.";
    return;
  }

  try {
    const response = await fetch("/create-url", {
      method: "POST",
      body: JSON.stringify({
        url: longUrl.value,
      }),
    });

    if (!response.ok) {
      error.value = "Por favor, ingresa un enlace válido.";
      return;
    }

    const data = await response.json();
    shortUrl.value = data["url"];

    showError.value = false;
    error.value = "";

  } catch (err) {
    error.value = "Error al procesar la solicitud.";
    console.error(err.message);
  }
}

async function copyToClipboard() {
  await navigator.clipboard.writeText(shortUrl.value)
}

</script>

<style scoped>

main {
  min-height: 100vh;
}

.main {
  padding-top: 110px;
  min-height: 100vh;
}

.info {
  border-bottom: 1px solid white;
  border-top: 1px solid white;

  color: white;

  text-align: center;

  padding-top: 3rem;
  padding-bottom: 3rem;

  h1 {
    font-weight: 900;
    font-size: 28pt;
    margin-bottom: 1rem;
  }

  p {
    font-size: 16pt;
    font-weight: 300;
  }
}

.ex {
  width: 100%;

  input {
    width: 100%;
  }
}

.error {
  color: red;
}

.banner-msg {
  color: var(--alt-background-color);
  text-align: center;

  h1 {
    font-weight: 900;
    font-size: 36pt;
    margin-bottom: 1rem;
  }

  p {
    font-size: 16pt;
    font-weight: 300;
  }

  margin-top: 6rem;
  margin-bottom: 6rem;
}

.shorter-form {
  background-color: var(--alt-background-color);
  padding: 25px;
  width: 650px;

  border-radius: 25px;

  h2 {
    color: var(--primary-color);
    margin-bottom: 0.8rem;
  }

  p {
    color: var(--muted-color);
    font-weight: 300;
  }
}

.shorter-form-input {
  margin-top: 1.8rem;
  display: flex;
  flex-direction: column;

  gap: 20px;

  input {
    height: 40px;
    font-size: 14pt;
    border-radius: 5px;
  }

  button {
    width: 95px;
    padding: 8px;
    border-radius: 12px;
    border: 0;
    background-color: var(--btn-color);
    color: var(--primary-color);
    font-weight: 900;
    font-size: 12pt;

    cursor: pointer;

    &:hover {
      box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 6px;
    }
  }
}

.container__form {
  margin-left: auto;
  margin-right: auto;

  width: 650px;
}
</style>
