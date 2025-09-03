<script>
export default {
  name: "HeaderComponent",
};
</script>

<template>
  <header>
    <nav>
      <div class="logo">
        <img src="/logo.png"
        alt="logo"
        height="65px"
        />
      </div>
      <div class="actions">
        <a class="login-btn">
          <span>Iniciar Sesion</span>
        </a>
        <button class="register-btn">Registrarme</button>
      </div>
    </nav>
  </header>
</template>

<style scoped>

header {
  position: absolute;
  top: 0;
  z-index: 1000;

  width: 100%;
}

nav {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px 20px;

  gap: 25vw;
}

.actions {
  display: flex;
  gap: 10px;
}

.login-btn {
  color: white;
  display: flex;

  justify-content: center;
  align-items: center;

  cursor: pointer;
}

.login-btn:hover {
  text-decoration: underline;
}

.register-btn {
  background: var(--btn-color);
  border: none;
  color: var(--primary-color);
  padding: 8px 16px;
  border-radius: 10px;
  font-weight: bold;

  cursor: pointer;

  font-size: 1rem;
}

.register-btn:hover {
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 6px;
  opacity: 0.8;
}

</style>
