<script>
export default {
  name: "FooterComponent",
};
</script>

<template>
  <footer class="footer">
    <div class="footer-content">
      <h1>Gestiona y Personaliza tus enlaces</h1>
      <p>
        registrate gratis y accede a tu panel de control donde podras gestionar y personalizar tus
        enlaces
      </p>
      <p>© 2025 ToyUrl. Todos los derechos reservados.</p>
      <button class="register-btn">Registrarme</button>
      <img src="C:\Users\user\Desktop\probando vue\origin\ToyURL-frontend\src\images\github-logo2.png" alt="">
    </div>
  </footer>
</template>

<style scoped>
.footer {
  margin-top: 40px;
  padding: 32px 0;
  background: #022341;
  color: white;
  display: flex;
  justify-content: center;
}

.footer-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 18px;
  width: 100%;
  max-width: 500px;
  text-align: center;
}

.register-btn {
  background: #43a047;
  color: white;
  border: none;
  padding: 10px 24px;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
  margin-top: 10px;
}

.github-logo {
  margin-top: 12px;
  height: 24px;
}
</style>
