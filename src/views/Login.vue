<template>
    <div>
        <Navbar/>
        <div class="container my-4">
  <form @submit.prevent="login( this.email, this.password )">  
    <div class="input-group mb-3">
    <span class="input-group-text">Correo</span>
    <input v-model="email" 
            type="email"
            required="true"
            class="form-control">
    </div>
    <!-- Familia -->
    <div class="input-group mb-3">
    <span class="input-group-text">Password</span>
    <input v-model="password" 
            type="password"
            required="true" 
            class="form-control">
    </div>
        <button type="submit" class="btn btn-primary">Registrar
    </button>
  </form>
  </div> 
    </div>
</template>

<script>
import { signInWithEmailAndPassword } from "firebase/auth";
import { auth } from "../main"
import Navbar from "../components/Navbar.vue"

export default {
    name: 'Login',
    components: {
        Navbar,
    },
    data() {
        return {
            email: '',
            password: '',
            errorMessage: ''
        };
    },
    methods: {
        login( email, password ) {
            signInWithEmailAndPassword(auth, email, password)
            .then((userCredential) => {
                alert('¡Sesión iniciada!');
                // Signed in
                const user = userCredential.user;
                this.$router.push('/');
                // ...
                })
            .catch((error) => {
            const errorCode = error.code;
            this.errorMessage = error.message;
            alert(this.errorMessage);
            });
        },
    },
};
</script>
