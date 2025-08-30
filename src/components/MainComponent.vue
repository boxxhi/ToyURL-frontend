<template>
  <main>
    <div class="form-container">
      <h2>Pega tu URL aqui</h2>
      <p>No requiere registro</p>
      <input v-model="longUrl" type="url" placeholder="URL" required />
      <button @click="acortarEnlace">Acortar</button>
    </div>
    <div v-if="shortUrl" class="result">
      <p>
        Enlace acortado: <a :href="shortUrl" target="_blank">{{ shortUrl }}</a>
      </p>
      <button @click="copiarAlPortapapeles">Copiar</button>
    </div>
    <div v-if="error" class="error">{{ error }}</div>
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
main {
  max-width: 600px;
  margin: 0 auto;
}

.form-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

input {
  padding: 10px;
  font-size: 16px;
}

button {
  padding: 10px;
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #3a9c72;
}

.result {
  margin-top: 20px;
  padding: 10px;
  background-color: #f0f0f0;
}

.error {
  color: red;
  margin-top: 10px;
}
</style>
