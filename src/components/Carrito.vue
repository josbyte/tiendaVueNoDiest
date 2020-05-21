<template>
  <div class="carrito">
    <br>
    <br>
    <br>
    <br>
    <br>
    <div>
      <h4 style="margin-left: 21em; display:inline-block;"><b>Artículo</b></h4>
      <h4 style="margin-left: 17em; display:inline-block;"><b>Precio</b></h4>
      <h4 style="margin-left: 6em; display:inline-block;"><b>Cantidad</b></h4>
      <h4 style="margin-left: 6em; display:inline-block;"><b>Subtotal</b></h4>
    </div>
    <div class="cardContainerList precio"  v-for="(producto, index) in carrito" :key="producto" >
      <div style="display: inline-block; margin-left: 18em; margin-right: -50em">
        <img :src="returnImage(carrito[index].img)" width="20%">
      </div>
      <div style="display: inline-block; word-wrap: break-word; width: 26em;">
        <h5>{{carrito[index].nombre}}</h5>
        <h5>{{carrito[index].esp}}</h5>
      </div>
      <div style="display: inline-block; width: 15em; vertical-align:top;">
        <h5 style="margin-left: 2em;">{{carrito[index].precio}}€</h5>
      </div>
      <div style="display: inline-block; width: 20em; vertical-align:top; margin-right: -100em">
        <h5 style="margin-left: 2em;">{{carrito[index].cantidad}}</h5>
      </div>
      <div style="display: inline-block; width: 10em; vertical-align:top; margin-right: -50em">
        <h5 style="margin-left: 2em;">{{carrito[index].precio*carrito[index].cantidad}}€</h5>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Carrito',
  props: {
  },
  data () {
    return {
      carrito: this.$parent.carrito,
      productos: this.$parent.productos,
      imagenes: this.$parent.imagenes
    }
  },
  mounted () {
    setTimeout(() => document.getElementById('loader').remove(), 1500)
    setTimeout(() => this.cargarImagen(), 3500)
  },
  methods: {
    quitarNotificacion: function () {
      document.getElementById('notificacion').style.display = 'none'
    },
    cargarImagen: function () {
      for (var j = 0; j < Object.keys(this.productos).length; j++) {
        var _this = this
        this.$parent.storage.ref('/').child(this.productos[j].img).getDownloadURL().then(function (url) {
          _this.traerImagen(url)
        })
      }
    },
    traerImagen: function (img) {
      this.imagenes.push(img)
    },
    returnImage: function (img) {
      for (var i = 1; i < this.imagenes.length; i++) {
        console.log(img)
        if (this.imagenes[i].includes(img)) {
          return this.imagenes[i]
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
