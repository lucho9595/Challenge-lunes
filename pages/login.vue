<template>
    <div class="login-container">
        <div class="login-form">
            <h1>Login</h1>
            <form @submit.prevent="login">
                <div class="form-group">
                    <input type="text" v-model="email" required>
                    <label for="emailInput" :class="{ 'active': email || emailFocused }">
                        Email:
                    </label>
                </div>
                <div class="form-group">
                    <input type="password" v-model="password" required>
                    <label for="passwordInput" :class="{ 'active': password || passwordFocused }">
                        Password:
                    </label>
                </div>
                <button type="submit" class="btn btn-danger">Submit</button>
            </form>
        </div>
    </div>
</template>

<script>
export default {
    name: "login",
    data() {
        return {
            email: '',
            password: '',
            emailFocused: false,
            passwordFocused: false
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
        },
    }
};
</script>


<style>
h1 {
    margin-bottom: 35px;
}

form {
    margin: 20px;
}

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
    position: relative;
}

input[type="text"],
input[type="password"] {
    width: 100%;
    padding: 10px 9px 10px;
    background: none;
    border: 2px solid white;
    outline: none;
    border-radius: 6px;
    font-size: 18px;
    color: white;
    margin-bottom: 25px;
}

label {
    position: absolute;
    top: 50%;
    left: 15px;
    font-size: 19px;
    transform: translateY(-100%);
    pointer-events: none;
    transition: 0.3s;
}

input:focus+label {
    top: 0px;
    left: 15px;
    font-size: 16px;
    padding: 0 2px;
    background: rgba(71, 71, 71, 0.21176);
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
