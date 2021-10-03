<template>
    <div>
        <Navbar/>
        <div class="container my-4">
  <form @submit.prevent="register( this.email, this.password )">  
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
    <!-- Descripción -->
    <div class="input-group mb-3">
    <span class="input-group-text">Repite Password</span>
    <input v-model="repassword" 
            type="password"
            required="true" 
            class="form-control">
    </div>
    <button type="submit" :disabled="!desactivar" class="btn btn-primary">Registrar
    </button>
  </form>
    <p>email {{ email }}</p>
  </div> 
    </div>
</template>
<script>
import { createUserWithEmailAndPassword } from "firebase/auth";
import { auth } from "../main"

import Navbar from "../components/Navbar.vue";

export default {
    name: 'Registro',
    components: {
        Navbar,
    },
    data() {
        return {
            email: '',
            password: '',
            repassword: '',
            errorMessage: ''
        };
    },
    methods: {
        register(email, password) {
            createUserWithEmailAndPassword(auth, email, password)
            .then((userCredential) => {
                // Signed in
                const user = userCredential.user;
                alert('¡Registrado!');
                // ...
            })
            .catch((error) => {
                const errorCode = error.code;
                this.errorMessage = error.message;
                alert(this.errorMessage);
                // ..
            });
        },        
    },
    computed: {
        desactivar(){
            return this.password === this.repassword
        }
    }
}
</script>
