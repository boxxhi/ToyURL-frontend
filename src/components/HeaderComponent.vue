<script>
export default {
  name: "HeaderComponent",
};
</script>

<template>
  <header class="header">
    <div class="logo">
      <img src="C:\Users\user\Desktop\probando vue\origin\ToyURL-frontend\src\images\Picture2.png" 
      alt="Logo" />
    </div>
    <div class="actions">
      <button class="login-btn">Iniciar Sesion</button>
      <button class="register-btn">Registrarme</button>
    </div>
  </header>
</template>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px;
  background-color: #12314f;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.logo img {
  height: 40px;
}

.actions {
  display: flex;
  gap: 10px;
}

.login-btn {
  background: transparent;
  border: 1px solid #1976d2;
  color: #1976d2;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
}

.register-btn {
  background: #28a745;
  border: none;
  color: white;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

</style>
