<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Vue 3 Firestore 9</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
         <li class="nav-item">
        <router-link class="nav-link active" aria-current="page" to="/">Home</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link active" aria-current="page" to="/productos">Productos</router-link>
        </li>
      </ul>
      <form class="d-flex">
        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-success me-2" type="submit">Buscar</button>
        <router-link class="btn btn-outline-primary me-2" to ="/login">Login</router-link>
        <button class="btn btn-outline-primary me-2"   @click="signout">Salir</button>
        <router-link class="btn btn-outline-danger me-2"  to ="/registro">Registro</router-link>
      </form>
    </div>
  </div>
</nav>
</template>

<script>
import { signOut } from "firebase/auth";
import { auth } from "../main";

export default {
  name: 'Navbar',
   methods: {
     signout () {
      signOut(auth).then(() => {
        alert('¡Sesión cerrada! Inicia sesión.');
        this.$router.push('/login');
        // Sign-out successful.
      }).catch((error) => {
        // An error happened.
      });
     }
   },
}
</script>

