<template>
<!-- ////////// formulario Añadir ////////// -->
    <!-- Nombre -->
  <div class="container my-4">
  <form>  
    <div class="input-group mb-3">
    <span class="input-group-text">Nombre</span>
    <input v-model="producto.nombre" type="text" class="form-control">
    </div>
    <!-- Familia -->
    <div class="input-group mb-3">
    <span class="input-group-text">Familia</span>
    <input v-model="producto.familia" type="text" class="form-control">
    </div>
    <!-- Descripción -->
    <div class="input-group mb-3">
    <span class="input-group-text">Descripción</span>
    <input v-model="producto.descripcion" type="text" class="form-control">
    </div>
    <!-- Precio -->
    <div class="input-group mb-3">
    <span class="input-group-text" id="inputGroup-sizing-default">Precio</span>
    <input v-model="producto.precio" type="text" class="form-control">
    </div>
    <!-- Valoración -->
    <div class="input-group mb-3">
    <span class="input-group-text" id="inputGroup-sizing-default">Valoración</span>
    <input v-model="producto.valoracion" type="text" class="form-control">
    </div>
    <!-- Unidades -->
    <div class="input-group mb-3">
    <span class="input-group-text" id="inputGroup-sizing-default">Unidades</span>
    <input v-model="producto.unidades" type="text" class="form-control">
    </div>
    <!-- Unidades -->
    <div class="input-group mb-3">
    <span class="input-group-text" id="inputGroup-sizing-default">Enlace</span>
    <input v-model="producto.enlace" type="text" class="form-control">
    </div>
    <!-- Cargar Imagen --> 
    <div class="input-group my-3">
        <input type="file" @change="buscarImagen($event)">
    </div>
    <div>
      <img :src="datoImagen">
    </div>
    <!-- Mostrar/Ocultar Botones Guardar-Editar -->
    <div class="mt-3">  
    <button v-show="this.editar === true" 
            @click.prevent="actualizarImagenDatos(id)" 
            class="btn btn-primary">Actualizar
    </button>
    <button v-show="this.editar === false" 
            @click.prevent="subirImagenDatos()" 
            class="btn btn-primary">Añadir
    </button>
    </div>
  </form>
  </div>
<!-- ////////// tabla ////////// -->
<div>
  <table class="table">
    <thead>
      <tr>
        <th scope="col">id</th>
        <th scope="col">Nombre</th>
        <th scope="col">Familia</th>
        <th scope="col">Precio</th>
        <th scope="col">Unidades</th>
        <th scope="col">Editar</th>
        <th scope="col">Elinimar</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in productos" :key="index">
        <th scope="row">{{index}}</th>
        <td>{{item.nombre}}</td>
        <td>{{item.foto}}</td>
        <td>{{item.precio}}</td>
        <td>{{item.id}}</td>
        <td>
          <button @click.prevent="obtenerDatoID( item.id );this.editar = !this.editar;" 
            class="btn btn-primary">Editar
          </button>
        </td>
        <td>
          <button @click.prevent="eliminarDato( item.id )" 
            class="btn btn-danger">Eliminar
          </button>
        </td>
      </tr>
    </tbody>
  </table>
</div>
</template>

<script>
// @ is an alias to /src
import { collection, getDocs, addDoc, doc, deleteDoc, getDoc, updateDoc } from 'firebase/firestore/lite';
import { db, storage } from "../main";
import router from '../router'

export default {
  name: 'Formulario',
  components: {
  },
  data() {
    return {
      editar: false,
      file: null,
      datoImagen: null,

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
  // GET
    async obtenerDatos () { 
      const data = await getDocs(collection(db, "productos"));
        data.forEach((doc) => {
        let producto = doc.data()
        producto.id = doc.id
        this.productos.push(producto)
        console.log(producto)
      });
    },
    // ADD / AGREGAR / AÑADIR
    async agregarDato( producto ){
      const docRef = await addDoc(collection(db, "productos"), {
        nombre: this.producto.nombre,
        descripcion: this.producto.descripcion,
        precio: this.producto.precio,
        valoracion: this.producto.valoracion,
        familia: this.producto.familia,
        unidades: this.producto.unidades,
        enlace: this.producto.enlace,
      })
        .then(() => {
          console.log("Documento añadido");
        })
        .catch(function(error) {
          console.error("Error al añadir el documento: ", error);
        });
    },
    // DELETE / ELIMINAR / BORRAR
    async eliminarDato (id){
      await deleteDoc(doc(db, "productos", id ));
      router.go('/productos');
    },
    // GET BY ID / OBTENER POR ID
    async obtenerDatoID (id){
      const docRef = doc(db, "productos", id);
      const docSnap = await getDoc(docRef);
          if (docSnap.exists()) {
            this.producto = docSnap.data()
            this.producto.id = docSnap.id
            } 
            else {
            console.log("¡No existe el documento!");
            }
    },
    // UPDATE / ACTUALIZAR
    async actualizarDato (producto) {
      const elemento = doc(db, "productos", this.producto.id );
      await updateDoc(elemento, {
        nombre: producto.nombre,
        descripcion: producto.descripcion,
        familia: producto.familia,
        precio: producto.precio,
        valoracion: producto.valoracion,
        unidades: producto.unidades,
        enlace: producto.enlace,
        foto: producto.foto
        })
        .then(() => {
        //router.push('Home');
        })
    },
    // BUSCAR IMAGEN
    buscarImagen(event){
      console.log(event.target.files[0]);
      const tipoArchivo = event.target.files[0].type;
        if(tipoArchivo === 'image/jpeg' || tipoArchivo === 'image/png'){
          this.file = event.target.files[0]
          this.error = null
        }
          else{
          this.error = 'Archivo no válido'
          this.file = null
          return 
          }
          const reader = new FileReader();
          reader.readAsDataURL(this.file);
          reader.onload = (e) => {
            this.datoImagen = e.target.result
            // console.log(e.target.result)
          }
    },
    // SUBIR IMAGEN STORAGE
    async subirImagenDatos(){
      try {
        this.loading = true
        const refImagen = storage.ref().child('imagenes').child(this.file.name)
        const res = await refImagen.put(this.file)
        console.log(res);
        const urlDescarga = await refImagen.getDownloadURL()
        await 
          addDoc(collection(db, "productos"), {
            nombre: this.producto.nombre,
            descripcion: this.producto.descripcion,
            precio: this.producto.precio,
            valoracion: this.producto.valoracion,
            familia: this.producto.familia,
            unidades: this.producto.unidades,
            enlace: this.producto.enlace,
            foto: urlDescarga
          })
          this.error = 'Imagen subida con éxito'
          this.file = null
      } 
        catch (error) {
          console.log(error);
        } 
        finally {
          router.push('/')
          this.loading = false
        }
      },
    async actualizarImagenDatos(){
      try {
        this.loading = true
        const refImagen = storage.ref().child('imagenes').child(this.file.name)
        const res = await refImagen.put(this.file)
        console.log(res);
        const urlDescarga = await refImagen.getDownloadURL()
        const elemento = doc(db, "productos", this.producto.id );
          await updateDoc(elemento, {
            nombre: this.producto.nombre,
            descripcion: this.producto.descripcion,
            precio: this.producto.precio,
            valoracion: this.producto.valoracion,
            familia: this.producto.familia,
            unidades: this.producto.unidades,
            enlace: this.producto.enlace,
            foto: urlDescarga
          })
          this.error = 'Imagen subida con éxito'
          this.file = null
      } 
        catch (error) {
          console.log(error);
        } 
        finally {
          router.push('/')
          this.loading = false
        }
      }        
  },
  mounted() {
    this.obtenerDatos();
  },
}
</script>