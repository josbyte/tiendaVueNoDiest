<template>
  <div id="home">
    <br>
    <br>
    <br>
    <br>
    <listaProductos/>
  </div>
</template>

<script>
import listaProductos from '../components/listaProductos'
export default {
  name: 'Home',
  components: {
    listaProductos
  },
  data () {
    return {
      carritoDinero: 0,
      carritoUnidad: 0,
      arrayProductos: this.$parent.productos,
      carrito: [{
      }],
      arrayImagenes: this.$parent.imagenes
    }
  },
  methods: {
    añadiProductoCarrito: function (index) {
      this.arrayProductos[index].stock--
      var added = false
      for (var i = 0; i < this.$parent.carrito.length; i++) {
        console.log(this.arrayProductos[index].nombre)
        console.log(this.arrayProductos[index].nombre === this.$parent.carrito[i].nombre)
        if (Object.values(this.$parent.carrito).indexOf(this.arrayProductos[index].nombre) > -1 && added === false) {
          console.log('11')
          this.$parent.carrito[i].cantidad++
          added = true
        }
        if (Object.values(this.$parent.carrito).indexOf(this.arrayProductos[index].nombre) === -1 && added === false) {
          console.log('22')
          this.$parent.carrito.push({ nombre: this.arrayProductos[index].nombre, cantidad: 0, precio: this.arrayProductos[index].precio, img: this.arrayProductos[index].img, esp: this.arrayProductos[index].esp })
          added = true
        }
      }
      console.log(this.arrayProductos[index].nombre)
      this.$parent.notificacion(this.arrayProductos[index].nombre)
      this.$parent.dineroCarrito()
    }
  },
  mounted () {
    console.log('aaa')
  }
}
</script>
<style>

#nav {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  background-color: #333333;
  padding: 15px;
  width: 100%;
  display: inline-block;
}

.navText{
  display: inline-block;
  font-size: 1.5em;
  margin-right: 70vh;
}

.butRouters{
  display: inline-block;
}

.routerButton{
  padding-left: 3vh
}

#nav a {
  font-weight: bold;
  color:  darkgrey;
  text-decoration: none;
}

#nav a.router-link-exact-active {
  color: white;
}

.greenDiv{
  background-color: green;
  display: inline-block;
  margin-left: 1em;
  padding-right: 1em;
  padding-top:0.8em;
  padding-bottom:0.8em;
  color:white;
  border-radius: 5px;
}

.whiteDiv{
  background-color: white;
  color: black;
  display: inline-block;
  padding-left: 0.5em;
  padding-right: 0.5em;
  border-radius: 4px;
}
</style>
