
<template>
  <div class="logo">
    <img src="/logo.png"
            alt="logo"
            height="65px"
            />
  </div>

  <div class="container">
    <div class="register-info">
      <h4>Registrate para empezar</h4>
      <a href="/login.html">¿Ya tienes cuenta? Inicia sesion aquí</a>
    </div>

    <div class="form-opts">
      <a class="google-btn">Continua con Google</a>
      <div class="separator">
        <div></div>
        <span>O</span>
        <div></div>
      </div>

      <div class="form">
        <div>
          <h3>Correo</h3>
          <input type="email" v-model="email" placeholder="" required id="email">
        </div>

        <div>
          <h3>Contraseña</h3>
          <input type="password" v-model="password" placeholder="" required id="password">
        </div>

        <div>
          <h3>Repita la contraseña</h3>
          <input type="password" v-model="repeatedPassword" placeholder="" required id="password">
        </div>

        <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
        <div v-if="successMessage" class="success">{{ successMessage }}</div>

        <button @click="register">Crear</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const email = ref('')
const password = ref('')
const repeatedPassword = ref('')
const errorMessage = ref('')
const successMessage = ref('')

async function register() {
  if (email.value == '') {
    return (errorMessage.value = 'Introduce un email')
  }

  if (password.value == '') {
    return (errorMessage.value = 'Introduce una contraseña')
  }

  if (repeatedPassword.value == '') {
    return (errorMessage.value = 'Repite la contraseña')
  }

  if (repeatedPassword.value !== password.value) {
    return (errorMessage.value = 'Las contraseñas no coinciden')
  }

  try {
    const response = await fetch("/api/register", {
      method: "POST",
      body: JSON.stringify({
        email: email.value,
        password: password.value,
      }),
      headers: {
        "Content-Type": "application/json",
      },
    });

    if (response.status > 401) {
      return (errorMessage.value = "Error interno. Por favor intente de nuevo");
    }

    const data = await response.json();

    /**
     * @type Boolean
     */
    const success = data['success']
    if (success) {
      errorMessage.value = ''

      const token = data['token']
      localStorage.setItem("token", token)

      successMessage.value = 'Redirigiendo...';
    } else {

      const errorsMap = {
        'already_used':"El correo ya esta en uso",
      }

      const message = data['message'] || "unknown"
      errorMessage.value = errorsMap[message]
    }

  } catch (err) {
    errorMessage.value = "Error al procesar la solicitud.";
    console.error(err.message);
  }
}

</script>

<style>

body {
  background-image: none;
  min-height: 100vh;
}

.container {
  width: 50%;
  margin-left: auto;
  margin-right: auto;
}

.logo {
  padding: 50px;
}

.register-info {
  h4 {
    color: var(--primary-color);
    font-size: 14pt;
  }

  p {
    color: var(--muted-color);
    font-weight: 400;
  }
}

a {
  text-decoration: none;
  color: var(--muted-color);

  &:hover {
    text-decoration: underline;
  }
}

.google-btn {
  width: 350px;
  padding: 8px;
  border-radius: 10px;
  background-color: #fff;
  text-align: center;

  border: 1px solid var(--muted-color);
  color: var(--primary-color);

  &:hover {
    text-decoration: none;
  }
}

.form-opts {
  margin-top: 80px;

  display: flex;
  flex-direction: column;

  justify-content: center;
  align-items: center;
}

.separator div:first-child {
   margin-right: 15px;
}

.separator div:last-child {
   margin-left: 15px;
}

.separator {
  margin-top: 15px;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;


  div {
    width: 150px;
    height: 1px;
    border: 1px solid var(--primary-color);
  }
}

.form {
  margin-top: 40px;
  border: 1px solid var(--primary-color);
  border-radius: 15px;
  display: flex;

  flex-direction: column;
  align-items: center;

  padding: 30px;

  font-size: 14px;

  color: var(--primary-color);

  gap: 15px;

  div:first-child {
      margin-top: 15px;
  }

  div {
    margin-left: 10px;
    margin-right: 10px;

  }

  div input {
    height: 30px;
    width: 225px;
    border-radius: 5px;

    border: 1px solid var(--muted-color);

    padding: 4px;

    outline: none;
  }

  button {
    margin-top: 20px;
    font-size: 12pt;
    font-weight: bold;
    width: 95px;

    padding: 5px;
    border-radius: 10px;
    background-color: var(--btn-color);
    border: none;

    &:hover {
      cursor: pointer;
    }

  }
}

.error {
  color: red;
}

.success {
  color: green;
}



</style>
