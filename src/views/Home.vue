<template>
<div>
  <Navbar/>
   <div class="container my-5">
            <div class="row row-cols-1 row-cols-md-3 g-4">
                <div class="col" v-for="(item, index) in productos" :key="index">
                    <div class="card">
                    <img :src= "item.foto" class="card-img-top">
                    <div class="card-body">
                        <h5 class="card-title"> {{item.nombre}}</h5>
                        <p class="card-text"> {{item.descripcion}}</p>
                    </div>
                    </div>
                </div>
            </div>
        </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "../components/Navbar.vue";
import { collection, getDocs } from 'firebase/firestore/lite';
import { db } from "../main";

export default {
  name: 'Home',
  components: {
        Navbar,
    },
  data() {
    return {
      productos: [],
        producto: {
            id: '',
            nombre: '',
            familia: '',
            descripcion: '',
            precio: '',
            valoracion: '',
            unidades: '',
            enlace: '',
            foto: ''}
    }
  },
  methods:{
    async obtenerDatos () { 
      const data = await getDocs(collection(db, "productos"));
        data.forEach((doc) => {
        let producto = doc.data()
        producto.id = doc.id
        this.productos.push(producto)
        console.log(producto)
      });
    }
  },
  mounted(){
    this.obtenerDatos();
  }
}
</script>