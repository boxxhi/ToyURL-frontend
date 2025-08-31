<template>
  <main>
    <div class="banner-msg">
      <h1>¡Empieza a acortar enlaces gratis!</h1>
      <p>Usa nuestro acortador para mejorar el diseño de tus links</p>
    </div>

    <div class="container">
      <div class="shorter-form">
        <h2>Pega tu URL aqui</h2>
        <p>No requiere registro</p>
        <div class="shorter-form-input">
          <input v-model="longUrl" type="url" placeholder="URL" required />
          <button @click="acortarEnlace">Acortar</button>
        </div>
      </div>
      <div v-if="shortUrl" class="result">
        <p>
          Enlace acortado: <a :href="shortUrl" target="_blank">{{ shortUrl }}</a>
        </p>
        <button @click="copiarAlPortapapeles">Copiar</button>
      </div>
      <div v-if="error" class="error">{{ error }}</div>
    </div>
  </main>
</template>

<script>
export default {
  name: "MainComponent",
  data() {
    return {
      longUrl: "",
      shortUrl: "",
      error: "",
    };
  },
  methods: {
    async acortarEnlace() {
      if (!this.longUrl) {
        this.error = "Por favor, ingresa un enlace válido.";
        return;
      }
      try {
        const response = await fetch(
          `https://toyUrl.com/api-create.php?url=${encodeURIComponent(this.longUrl)}`
        );
        if (!response.ok) {
          throw new Error("Error al acortar el enlace");
        }
        const data = await response.text();
        this.shortUrl = data;
        this.error = "";
      } catch (err) {
        this.error = err.message;
        this.shortUrl = "";
      }
    },
    copiarAlPortapapeles() {
      navigator.clipboard
        .writeText(this.shortUrl)
        .then(() => {
          alert("Enlace copiado al portapapeles");
        })
        .catch((err) => {
          console.error("Error al copiar:", err);
        });
    },
  },
};
</script>

<style scoped>

.banner-msg {
  margin-top: 50px;
  color: var(--alt-background-color);
  text-align: center;

  h1 {
    font-weight: 900;
    font-size: 36pt;
    margin-bottom: 1rem;
  }

  p {
    font-size: 16pt;
    opacity: 0.85;
    line-height: 1.25rem;
  }

  margin-bottom: 4rem;
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
  }
}

.container {
  margin-left: auto;
  margin-right: auto;

  width: 650px;
}

</style>
