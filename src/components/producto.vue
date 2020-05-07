<template>
  <div class="producto">
    <v-dialog v-model="galeria" width="800">
      <v-card width="800">
     <!--   <v-carousel>
          <v-carousel-item
            v-for="(item, i) in producto.image"
            :key="i"
            :src="item.src"
            :aspect-ratio="16 / 12"
            reverse-transition="fade-transition"
            transition="fade-transition"
          ></v-carousel-item>
        </v-carousel> -->

         <v-carousel :show-arrows="false">
            <v-carousel-item :src="producto.url"></v-carousel-item>
            <v-carousel-item :src="producto.url2"></v-carousel-item>
            <v-carousel-item :src="producto.url3"></v-carousel-item>
          </v-carousel>
      </v-card>
    </v-dialog>
    <br />
    <br />
    <v-hover v-slot:default="{ hover }">
      <v-card
        class="mx-auto"
        color="grey lighten-4"
        max-width="500"
        height="700"
      >
         <img max-width="100" height="300"  v-bind:src="producto.url2" />
        <div v-for="(item, i) in producto.image" :key="i">
          <div v-if="i == 1">
            <img width="150" height="100" :aspect-ratio="16 / 9" v-bind:src="item.src" />
          </div>
        </div>

        <v-expand-transition>
          <div
            v-if="hover"
            class="d-flex transition-fast-in-fast-out orange darken-2 v-card--reveal display-3 white--text"
            style="height: 100%;"
          >
            <p class="font-weight-green">
              <br /><br /><br />
              <br /><br /><br />
              <br />
              A solo $ {{ producto.precio }}
            </p>
          </div>
        </v-expand-transition>

        <v-card-text class="pt-6" style="position: relative;">
          <v-btn
            absolute
            color="orange"
            class="white--text"
            fab
            large
            left
            top
            @click="cont--, dec(), mercadopag()"
            v-if="cont > 0"
          >
            <v-icon>remove_shopping_cart</v-icon>
          </v-btn>
          <v-btn fab color="cyan darken-2" @click="gal()">
            <v-icon>photo_library </v-icon>
          </v-btn>
          <v-btn
            absolute
            color="orange"
            class="white--text"
            fab
            large
            right
            top
            @click="cont++, addToCart()"
            v-if="qtyCart === 0"
          >
            <v-icon>add_shopping_cart</v-icon>
          </v-btn>
          <v-btn
            absolute
            color="orange"
            class="white--text"
            fab
            large
            right
            top
            @click="cont++, inc()"
            v-else
          >
            <v-icon>add_shopping_cart</v-icon>
          </v-btn>
          <div class="font-weight-light orange--text title mb-2">
            Siempre la mejor calidad
          </div>

          <h3 class="display-1 font-weight-light orange--text mb-2">
            {{ producto.nombre }}
            
          </h3>
          <p class="font-weight-black orange--text mb-4">
            $ {{ producto.precio }}
          </p>
          <div class="font-weight-light title mb-2 orange--text mb-2">
            Envios sin cargo<br />
            <br />
            <br />
            <div v-if="cont > 0">
              <h4 class="display-1 font-weight-light orange--text mb-2">
                Productos {{ cont }} en el
                <v-icon x-large dark color="orange"
                  >local_grocery_store
                </v-icon>
              </h4>
            </div>
          </div>
        </v-card-text>
      </v-card>
    </v-hover>
  </div>
</template>

<script>
import logica from "../logica";

export default {
  props: ["producto"],
  data() {
    return {
       imag1: "",
    imag2: "",
    imag3: "",
      galeria: false,
      cont: 0,
      aux:'',
      shared: logica.data,
    };
  },
  computed: {
    qtyCart() {
      var busqueda = _.find(this.shared.cart, ["id", this.producto.id]);
      if (typeof busqueda == "object") {
        return busqueda.qty;
      } else {
        return 0;
      }
    },
  },
 
  methods: {
     zoon(ima1, ima2, ima3) {
      this.imag1 = ima1;
      this.imag2 = ima2;
      this.imag3 = ima3;
      this.dialog2 = true;
    
    },
    gal(){
          this.galeria=true
         
    },
 
    addToCart() {
      logica.add(this.producto);
    },
    inc() {
      logica.inc(this.producto);
 
    },
    dec() {
      logica.dec(this.producto);
    },
  },
};
</script>
<style>
.v-card--reveal {
  align-items: center;
  bottom: 0;
  justify-content: center;
  opacity: 0.5;
  position: absolute;
  width: 100%;
}
.fondo {
  background-color: rgb(207, 204, 181);

  margin: 0 auto;
}
.producto {
  background: rgb(207, 204, 181);
  margin: 0.5em;
  float: right;
  text-align: center;
  margin-bottom: 0.5em;
  align-content: center;
}
</style>
