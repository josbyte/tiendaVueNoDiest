<template>
  <div id="app">
    <div id="nav" style="position: fixed; z-index: 10;">
      <div class="navText">
        <router-link class="routerButton" to="/Home" style="color:white;font-weight:normal">Tienda Online</router-link>
      </div>
      <div class="butRouters">
        <router-link class="routerButton" to="/Login">Login</router-link>
        <router-link class="routerButton" to="/Registro">Registro</router-link>
        <div class="greenDiv">
        <router-link class="routerButton" to="/Carrito" style="color:white;">Carrito</router-link>
          &nbsp;
          <div class="whiteDiv">
            {{carritoUnidad}} - ({{carritoDinero}}€)
          </div>
        </div>
      </div>
    </div>
    <div id="loader" style="position: fixed; width: 100%; height: 100%; top: 5em; padding:0; margin:0; background-color: white; z-index: 3;">
      <pulse-loader :loading="loading" :color="color" :size="size" style="position: fixed; top: 50%; left: 50%; z-index: 2;"></pulse-loader>
    </div>
    <div id="notificacion" style="position: fixed; display: none; left: 37%; background-color: green; padding: 0.5em; z-index: 3; color: white;">
    </div>
    <router-view/>
  </div>
</template>
<script>
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'
import Firebase from 'firebase'
import { mapGetters } from 'vuex'
const config = {
  apiKey: 'AIzaSyDnL-io-T40zzIIIltfzxfOsLo6yc7ZyqY',
  authDomain: 'tiendavue-a3592.firebaseapp.com',
  databaseURL: 'https://tiendavue-a3592.firebaseio.com',
  projectId: 'tiendavue-a3592',
  storageBucket: 'tiendavue-a3592.appspot.com',
  messagingSenderId: '951077368182',
  appId: '1:951077368182:web:332573b939cfba5608ca57'
}
const app = Firebase.initializeApp(config)
const db = app.database()
var storage = Firebase.storage()
export default {
  name: 'App',
  components: {
    PulseLoader
  },
  data () {
    return {
      carritoDinero: 0,
      carritoUnidad: 0,
      carrito: [{
      }],
      db: db,
      storage: storage,
      productos: [{}],
      imagenes: [{}]
    }
  },
  mounted () {
    setTimeout(() => document.getElementById('loader').remove(), 1500)
    setTimeout(() => this.db.ref('productos').on('value', snapshot => this.cargaProductos(snapshot.val())), 100)
  },
  methods: {
    dineroCarrito: function () {
      var total = 0
      for (var i = 1; i < this.carrito.length; i++) {
        total += this.carrito[i].precio
      }
      this.carritoDinero = total
      this.carritoUnidad += 1
    },
    cargaProductos: function (productos) {
      this.productos = productos
      this.cargarImagen()
    },
    cargarImagen: function () {
      for (var j = 0; j < Object.keys(this.productos).length; j++) {
        var _this = this
        this.storage.ref('/').child(this.productos[j].img).getDownloadURL().then(function (url) {
          _this.traerImagen(url)
        })
      }
    },
    traerImagen: function (img) {
      this.imagenes.push(img)
    },
    notificacion: function (nombre) {
      document.getElementById('notificacion').innerHTML = ''
      var button = document.createElement('button')
      button.setAttribute('type', 'button')
      button.setAttribute('id', 'btnCerrar')
      button.setAttribute('class', 'btn')
      document.getElementById('notificacion').style.display = ''
      document.getElementById('notificacion').append('Has añadido ' + nombre + ' al carrito')
      document.getElementById('notificacion').append(button)
      document.getElementById('btnCerrar').style.color = 'white'
      document.getElementById('btnCerrar').innerHTML = '<b>x</b>'
      setTimeout(() => this.quitarNotificacion(), 3000)
    },
    quitarNotificacion: function () {
      document.getElementById('notificacion').style.display = 'none'
    }
  },
  computed: {
    ...mapGetters({
      user: 'user'
    })
  }
}
</script>
