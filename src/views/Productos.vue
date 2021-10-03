<template>
  <div>
    <Navbar/>
    <Formulario/>
  </div>
</template>

<script>
import Formulario from '../components/Formulario.vue'
import Navbar from "../components/Navbar.vue";

export default {
  name: 'Productos',
  components: {
    Formulario,
    Navbar
  },
}
</script>
