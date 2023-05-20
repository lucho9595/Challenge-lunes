<template>
    <div class="login-container">
        <div class="login-form">
            <h1>Login</h1>
            <form @submit.prevent="login">
                <div class="form-group">
                    <label :class="{ 'active': email !== '' }">
                        <span>Email:</span>
                        <input type="text" v-model="email" required>
                    </label>
                </div>
                <div class="form-group">
                    <label :class="{ 'active': password !== '' }">
                        <span>Password:</span>
                        <input type="password" v-model="password" required>
                    </label>
                </div>
                <button type="submit" class="btn btn-danger">Submit</button>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            email: '',
            password: ''
        };
    },
    methods: {
        login() {
            // Verificar las credenciales ingresadas
            if (this.email === 'admin@admin.com' && this.password === 'admin123') {
                // Inicio de sesión exitoso, redirige al componente de películas
                this.$router.push('/movie');
            } else {
                // Mostrar mensaje de error si el inicio de sesión falló
                alert('Inicio de sesión fallido. Por favor, verifica tus credenciales.');
            }
        }
    }
};
</script>

<style>
.login-container {
    background-color: #000;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}

.login-form {
    background-color: #47474736;
    padding: 58px;
    border-radius: 4px;
    text-align: center;
    color: white;
}

.form-group {
    margin-bottom: 15px;
}

label {
    position: relative;
    display: block;
    color: white;
}

label span {
    position: absolute;
    left: 10px;
    top: 22px;
    font-size: 14px;
    transition: top 0.3s, font-size 0.3s;
}

label.active span {
    top: 6px;
    font-size: 12px;
}

input[type="text"],
input[type="password"] {
    width: 100%;
    padding: 30px 10px 10px;
}

.btn-danger {
    background-color: #e50914;
    border-color: #e50914;
    color: #fff;
}

.btn-danger:hover {
    background-color: #c90c1e;
    border-color: #c90c1e;
}
</style>
