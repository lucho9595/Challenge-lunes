<template>
  <div class="landing-page">
    <div class="content">
      <h1>Bienvenido a nuestro sitio de películas</h1>
      <p>Descubre las mejores películas y encuentra toda la información que necesitas.</p>
      <router-link to="/login" class="btn btn-primary">Iniciar sesión</router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage'
}
</script>

<style>
.landing-page {
  background-image: url("https://images.unsplash.com/photo-1536440136628-849c177e76a1?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1025&q=80");
  background-size: cover;
  background-position: center;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.content {
  text-align: center;
  color: #fff;
}

.content h1 {
  font-size: 3rem;
  margin-bottom: 1rem;
}

.content p {
  font-size: 1.5rem;
  margin-bottom: 2rem;
}

.btn-primary {
  background-color: red;
  color: #fff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.btn-primary:hover {
  background-color: #c80000;
}
</style>
