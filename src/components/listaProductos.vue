<template>
  <div class="listaProductos">
    <div style='margin-left: 100vh;'>
      <button class="btn btn-outline-dark" @click="tipoLista = 'cuadrado'" style="float:right;  margin-right:30vh;">Cuadricula</button>
      <button class="btn btn-outline-dark" @click="tipoLista = 'lista'" style="float:right;">Lista</button>
      <br>
    </div>
    <div class="container mt-3 d-flex align-items-stretch" v-if="tipoLista == 'cuadrado'">
      <div class="cardContainer "  v-for="producto in arrayProductos" :key="producto" >
        <div class="row">
          <div class="col-auto mb-5 d-flex align-items-stretch">
            <div class="card card-box" style="width: 21em; height: 37em;">
              <div class="card-body">
                <img class="imgProducto" :src="returnImage(producto.img)">
                <br>
                <br>
                <br>
                <a href="#" @click="cargarDetalles(producto.img, producto.nombre, producto.esp, producto.precio, producto.stock)">{{producto.nombre}}</a>
                <p style="color:red">Quedan {{producto.stock}} artículos en stock</p>
                <p class="card-text">{{producto.esp}}</p>
                <div>
                  <h5 class="precio">{{producto.precio}}€</h5>
                  <a class="comprarBtn btn btn-primary" @click="anadirCarrito(producto.nombre)" v-if="producto.stock != 0">Añadir carrito</a>
                  <button disabled class="comprarBtnDisabled btn" v-if="producto.stock == 0">Agotado</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="container mt-5  " v-if="tipoLista == 'lista'">
      <div class="cardContainerList precio"  v-for="producto in arrayProductos" :key="producto" >
        <div class="row">
          <div class="card card-list " style="width: 101em; height: 14em;">
            <div class="card-body">
              <img class="imgProductolista" :src="returnImage(producto.img)" style="float: left;">
              <h5><a href="#" @click="cargarDetalles(producto.img, producto.nombre, producto.esp, producto.precio, producto.stock)" style="margin-left: 1.5em;">{{producto.nombre}}</a></h5>
              <div>
                <p class="precio" style="color:red; padding-left: 1.8em;">Quedan {{producto.stock}} artículos en stock</p>
              </div>
            </div>
              <p class="card-text-list">{{producto.esp}}</p>
              <h5 class="precio" style="padding-left: 13.5em; padding-top: 1.5em;">{{producto.precio}}€</h5>
              <a href="#" @click="anadirCarrito(producto.nombre)" class="comprarBtn-lista btn btn-primary" v-if="producto.stock != 0">Añadir carrito</a>
              <button disabled class="comprarBtnDisabled-lista btn" v-if="producto.stock == 0">Agotado</button>
          </div>
        </div>
      </div>
    </div>
    <div class="container mt-5  " v-if="tipoLista == 'productoDetalle'">
      <div class="card card-list " style="width: 70em; height: 33em;">
          <div class="card-body">
            <img class="imgProductoDetalle" :src="returnImage(this.detallesObjeto[0])" style="padding-left: 20em;" width="800vh" height="300vh">
            <br>
            <br>
            <div>
              <h3 class="card-title precio" style="padding-left: 0em;">{{this.detallesObjeto[1]}}</h3>
              <h5 class="precio" style="padding-left: 27em; padding-top: 1.5em;">{{this.detallesObjeto[3]}}€</h5>
            </div>
            <br>
            <p class="card-text precio">{{this.detallesObjeto[2]}}</p>
            <div class="precio">
              <p style="color:red; marging-bottom: 1em; precio">Quedan {{this.detallesObjeto[4]}} artículos en stock</p>
              <button disabled class="comprarBtnDisabled btn precio" style="margin-left: 58em; margin-top: 2vh; margin-right: 1em; margin-bottom: 1em;" v-if="this.detallesObjeto[4] == 0">Agotado</button>
              <a href="#" class="comprarBtn btn btn-primary" style="margin-left: 58em; margin-top: 2vh; margin-right: 1em; margin-bottom: 1em;" v-if="this.detallesObjeto[4] != 0">Añadir carrito</a>
            </div>
          </div>
      </div>
      <button class="btn btnVolver"  @click="tipoLista = 'cuadrado'">Volver</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'listaProductos',
  props: {
  },
  data () {
    return {
      test: '',
      tipoLista: 'cuadrado',
      detallesObjeto: [],
      arrayProductos: this.$parent.$parent.productos,
      arrayImagenes: this.$parent.$parent.imagenes
    }
  },
  mounted () {
    setTimeout(() => this.updateStock(), 100)
    setTimeout(() => console.log(this.arrayProductos), 100)
  },
  methods: {
    returnImage: function (img) {
      for (var i = 1; i < this.arrayImagenes.length; i++) {
        if (this.arrayImagenes[i].includes(img)) {
          return this.arrayImagenes[i]
        }
      }
    },
    cargarDetalles: function (img, nombre, esp, precio, stock) {
      this.detallesObjeto = []
      this.detallesObjeto.push(img)
      this.detallesObjeto.push(nombre)
      this.detallesObjeto.push(esp)
      this.detallesObjeto.push(precio)
      this.detallesObjeto.push(stock)
      this.tipoLista = 'productoDetalle'
    },
    anadirCarrito: function (nombre) {
      for (var i = 0; i < this.arrayProductos.length; i++) {
        if (this.arrayProductos[i].nombre === nombre) {
          this.$parent.añadiProductoCarrito(i)
        }
      }
    },
    updateStock: function () {
      this.arrayProductos = this.$parent.$parent.productos
      this.arrayImagenes = this.$parent.$parent.imagenes
      var carrito = this.$parent.$parent.carrito
      if (carrito.lenght !== 0) {
        for (var i = 1; i < carrito.length + 3; i++) {
          for (var j = 0; j < Object.keys(this.$parent.$parent.arrayProductos).length; j++) {
            if (this.$parent.arrayProductos[j].nombre === this.$parent.$parent.carrito[i].nombre) {
              this.$parent.$parent.arrayProductos[j].stock--
              console.log('aaa')
            }
          }
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
a{
  background-color: white;
  color:green;
}
a:hover {
  color: lightgreen;
}
h3 {
  margin: 40px 0 0;
}
.imgProducto{
  width: 18em;
  height: 15em;
  transition: transform .2s;
}
.imgProducto:hover {
  transform: scale(1.1);
}
.imgProductolista{
  display: inline-block;
  width: 14em;
  height: 11em;
  transition: transform .2s;
}
.imgProductolista:hover {
  transform: scale(1.1);
}
.imgProductoDetalle{
  transition: transform .2s;
}
.imgProductoDetalle:hover {
  transform: scale(1.1);
}
.precio{
  display: inline;
}
.comprarBtn{
  margin-left:  7em;
  color: white;
  background-color: green;
}
.comprarBtnDisabled {
  margin-left:  7em;
  color: grey;
  background-color: #35532f;
}
.comprarBtn-lista{
  margin-left:  60em;
  margin-right: 2em;
  margin-bottom: 1em;
  color: white;
  background-color: green;
}
.comprarBtnDisabled-lista{
  margin-left:  60em;
  margin-right: 2em;
  margin-bottom: 1em;
  color: grey;
  background-color: #35532f;
}
.productContainer{
  width: 100%;
}
.cardContainer{
  display: inline-block;
  padding-left: 2em;
}
.cardContainerList{
  padding-left: 2em;
}
.card-list{
  display: normal;
}
.card-box{
  display: inline-block;
}
.card-body-lista{
  display: inline-block;
}
.card-text-list{
  padding-left:17em;
  margin-bottom: -1em;
}
.card-title{
  color: #1ebf02;
  padding-left: 3em;
}
.card-title-lista{
  color: #1ebf02;
  padding-left: 1.5em;
  display: inline-block;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}

</style>
