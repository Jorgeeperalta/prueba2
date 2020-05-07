<template>
  <v-app>
    <div class="text-xs-center">
      <v-menu
        min-width="300"
        top
        v-model="menu"
        :close-on-click="false"
        :close-on-content-click="false"
        :nudge-top="60"
        offset-x
      >
        <template v-slot:activator="{ on }">
          <v-fab-transition>
            <v-btn
              v-model="fab"
              v-on="on"
              color="green darken-1"
              @click="
                fab = !fab;
                menu = false;
              "
              dark
              fixed
              bottom
              right
              fab
            >
              <v-icon>comment</v-icon>
            </v-btn>
          </v-fab-transition>
        </template>
        <v-card>
          <v-list dark class="chat-header">
            <v-list-tile>
              <v-list-tile-content>
                <v-list-tile-title>Enviame un WhatsApp</v-list-tile-title>
              </v-list-tile-content>
              <v-list-tile-action>
                <v-btn
                  icon
                  @click="
                    fab = !fab;
                    menu = false;
                  "
                >
                  <v-icon>cancel</v-icon>
                </v-btn>
              </v-list-tile-action>
            </v-list-tile>
          </v-list>
          <v-divider></v-divider>
          <v-container class="chat-background">
            <v-layout row wrap>
              <v-flex class="text-xs-right" xs12 mb-4>
                <v-chip class="chip-chat ">
                  Hola, en que puedo ayudarte? <v-icon right>done_all</v-icon>
                </v-chip>
              </v-flex>
              <v-flex xs10>
                <v-text-field
                  single-line
                  color="teal"
                  class="my-input"
                  v-model="text2"
                  outline
                ></v-text-field>
              </v-flex>
              <v-flex xs2>
                <v-btn
                  large
                  class="teal--text"
                  icon
                  @click="
                    fab = !fab;
                    menu = false;
                  "
                  :href="apilink"
                  target="_blank"
                >
                  <v-icon large>send</v-icon>
                </v-btn>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card>
      </v-menu>
    </div>

    <v-dialog v-model="dialog2">
      <!-- buscador de categorias-->

      <v-card
        class="mx-auto"
        height="450"
        width="256"
        color="blue-grey darken-3"
      >
        <!--
        <div class="pa-2">
          <v-btn color="purple accent-3" block @click="seleccionar(1)"
            >Limpieza</v-btn
          >
        </div>
        <div class="pa-2">
          <v-btn color="green" block @click="seleccionar(2)">Verduras</v-btn>
        </div>
        <div class="pa-2">
          <v-btn color="blue" block @click="seleccionar(3)">Almacen</v-btn>
        </div>
        <div class="pa-2">
          <v-btn color="red" block @click="dialog2 = false">salir</v-btn>
       
        </div>
        -->
        <v-img
          class="white--text align-end"
          height="200px"
          width="256"
          src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
        >
        </v-img>
        <v-spacer></v-spacer> <br />
        <v-label>Seleccione una categoria</v-label>
        <select v-model="eleccion" class="select-css">
          <option
            v-for="(item, index) in items2"
            :key="index"
            v-bind:value="item.nombre"
          >
            {{ item.nombre }}
          </option>
        </select>

        <div class="pa-2">
          <v-btn color="blue" block @click="seleccionar(eleccion)"
            >Buscar</v-btn
          >
        </div>
        <div class="pa-2">
          <v-btn color="red" block @click="dialog2 = false">salir</v-btn>
        </div>
      </v-card>
    </v-dialog>
    <template>
      <v-row>
        <v-dialog v-model="dialog" width="500px" >
          <!-- carrito de compra -->
          <v-card dark color="black" >
            <v-card-title>
              <span class="headline"></span>
            </v-card-title>
            <carrito></carrito>
            <div >
              <v-spacer></v-spacer>
              <div> 
              <v-flex xs10>
                <v-text-field
                  single-line
                  outline
                  color="teal"
                  v-model="nombre"
                  label="**Nombre"
                ></v-text-field>
              </v-flex>
              <br />
              
              <v-flex xs10>
                <v-text-field
                  label="**Direccion"
                  single-line
                  color="teal"
                  class="my-input"
                  v-model="text"
                  outline
                ></v-text-field>
              </v-flex>
              </div>
                <v-btn
                  large
                  class="teal--text"
                  icon
                  @click="
                    fab = !fab;
                    menu = false;
                    dialog = false;

                    abrir = true;
                  "
                  :href="apilink"
                  target="_blank"
                >
                     Enviar pedido via Whatsapp -----
                  <v-icon large>send</v-icon>
                  
                </v-btn>
              <br>
       
              <v-spacer></v-spacer>

             

              <v-btn color="red" @click="dialog = false"
                >Elimina <v-icon>delete_forever</v-icon></v-btn
              >
            </div>
          </v-card>
        </v-dialog>
      </v-row>
    </template>

    <v-toolbar color="light-green darken-3" height="15px">
      <template v-slot:extension>
        <v-btn
          color="cyan darken-2"
          fab
          x-large
          dark
          @click="(dialog2 = true), cargar_datos()"
        >
          <v-icon>format_list_bulleted</v-icon></v-btn
        >
        <v-spacer></v-spacer> <v-spacer></v-spacer>
        <v-btn
          v-if="bandera == true"
          bottom
          right
          color="orange"
          fab
          x-large
          dark
          @click="cambiar(), mercadopag()"
        >
          <v-icon>local_grocery_store</v-icon>
        </v-btn>
      </template>
    </v-toolbar>

    <div v-if="bandera == false">
      <img
        height="800"
        src="https://cdn.pixabay.com/photo/2017/08/10/07/20/grocery-store-2619380_960_720.jpg"
      />
    </div>

    <div v-if="bandera == true" height="800">
      <producto
        v-for="producto in productos"
        :producto="producto"
        :key="producto.id"
      ></producto>
    </div>
  </v-app>
</template>

<script>
import producto from "@/components/producto.vue";
import carrito from "@/components/carrito.vue";

import logica from "../logica";
import _ from "lodash";
import catalogo from "@/components/Catalogo.vue";

export default {
  components: {
    producto,
    carrito,
  },
/////
  data() {
    return {
      total_productos2: [
        {
          id: "",
          categoria: "",
          nombre: "",
          precio: "",
          imagenes: [
            {
              id_imagen: "",
              src: "",
              id: "",
            },
          ],
        },
      ],

      aux: "",
      abrir: false,
      bandera: false,
      dialog2: false,
      articulos: "",
      precio: "",
      cantidad: "",
      canasta: logica.data.cart,
      fab: false,
      fav: true,
      menu: false,
      message: false,
      hints: true,
      phone: "5491121655482",
      text: "",
      text2: "",
      apilink: "",
      items2: "",
      dialog: false,
      drawer: null,
      eleccion: "",
      nombre: "",
      direccion: "",

      items1: [
        { title: "Home", icon: "dashboard" },
        { title: "About", icon: "question_answer" },
      ],
      items: [
        {
          color: "#1F7087",
          src: "https://cdn.vuetifyjs.com/images/cards/foster.jpg",
          title: "Mostar carrito",
          artist: "Mostrar carrito",
        },
        {
          color: "#952175",
          src: "https://cdn.vuetifyjs.com/images/cards/halcyon.png",
          title: "Comprar",
          artist: "Ellie Gouldingc",
        },
      ],
      total_pruductos: [],
      productos: [],
    };
  },

  watch: {
    text2(val) {
      this.apilink = "http://";
      this.apilink += this.isMobile() ? "api" : "web";
      this.apilink +=
        ".whatsapp.com/send?phone=" +
        this.phone +
        "&text=" +
        encodeURI(this.text2);
    },
    text(val) {
      this.apilink = "http://";
      this.apilink += this.isMobile() ? "api" : "web";
      this.apilink +=
        ".whatsapp.com/send?phone=" +
        this.phone +
        "&text=" +
        encodeURI(
          "Mi nombre es " +
            this.nombre +
            " vivo en la calle " +
            this.text +
            " y mi pedido es el siguiente: " +
            this.articulos +
            " total aproximado es $ " +
            this.total
        );
    },
  },
  mounted() {
    this.llamar_categorias();
  },
  created: function() {
    this.apilink = "http://";
    this.apilink += this.isMobile() ? "api" : "web";
    this.apilink +=
      ".whatsapp.com/send?phone=" +
      this.phone +
      "&text=" +
      encodeURI("hola esta es una prueba!");
  },
  computed: {
    total() {
      return _.sumBy(this.canasta, function(it) {
        return it.precio * it.qty;
        this.cantidad = it.qty;
      });
    },
  },

  methods: {
    llamar_categorias() {
      var cont = 0;
      async function asyncData() {
        const response = await fetch(
          "http://localhost:81/Demo_carro/src/backend/catalogo.php"
        );
        const data = await response.json();

        return data;
      }

      const result = asyncData();

      result.then((data) => {
        this.items2 = data;

        console.log(this.items2);
      });
    },
    cargar_datos() {
      /////////////////////////
      /*var cont = 0;
      async function asyncData() {
        const response = await fetch(
          "http://jorgeperalta-001-site6.itempurl.com/post.php"
        );
        const data = await response.json();

        return data;
      }

      const result = asyncData();

      result.then((data) => {
        data.forEach((element) => {
          this.total_productos2[cont] = element;
          cont++;
        });
      });
      ///////////////////////
       fetch("http://jorgeperalta-001-site6.itempurl.com/post.php")
        .then((datos) => datos.json())
        .then((datos) => {
          //  console.log(datos);
          this.total_productos2 = datos;
        });

      fetch("http://jorgeperalta-001-site6.itempurl.com/post_imag.php")
        .then((datos) => datos.json())
        .then((datos) => {
          this.total_productos2.imagenes = datos;
        }); /*
    },

    mercadopag() {
      /* let foo = document.createElement("script");
      foo.setAttribute(
        "src",
        "https://www.mercadopago.com.br/integrations/v1/web-tokenize-checkout.js"
      );
      foo.setAttribute("data-transaction-amount", "14.90");
      this.$refs.myform.appendChild(foo);*/
    },

    seleccionar(valor) {
      this.productos = [];
      var cont = 0;
      var requestOptions = {
        method: "GET",

        redirect: "follow",
      };

      fetch(
        "http://localhost:81/Demo_carro/src/backend/articulos.php?categoria=" +
          valor,
        requestOptions
      )
        .then((response) => response.json())
        .then((result) => console.log((this.productos = result)))
        .catch((error) => console.log("error", error));

      /*
      for (var i = 0; i < this.total_productos2.length; i++) {
        if (valor == this.total_productos2[i].categoria) {
          for (var j = 0; j < this.total_productos2.imagenes.length; j++) {
            if (
              this.total_productos2[i].id ==
              this.total_productos2.imagenes[j].id
            ) {
              this.productos.push({
                id: this.total_productos2[i].id,
                categoria: this.total_productos2[i].categoria,
                nombre: this.total_productos2[i].nombre,
                precio: this.total_productos2[i].precio,

                image: [
                  { src: this.total_productos2.imagenes[j].src },

                  { src: this.total_productos2.imagenes[j + 1].src },

                  { src: this.total_productos2.imagenes[j + 2].src },
                ],
              });

              break;
            }
          }
        }
      }

      //console.log(this.productos);
         (this.productos = []),
        this.total_pruductos.forEach((element) => {
          if (element.categoria == valor) {
            this.productos.push({
              id: element.id,
              nombre: element.nombre,
              precio: element.precio,
              image: element.image,
            });
          }
        });*/

      this.bandera = true;
      this.dialog2 = false;
    },
    cambiar() {
      this.canasta.forEach((element) => {
        this.articulos =
          " " +
          this.articulos +
          "cant:  " +
          element.qty +
          " " +
          element.nombre +
          ", ";
      });
      this.dialog = true;
    },
    isMobile() {
      var check = false;
      (function(a) {
        if (
          /(android|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|mobile.+firefox|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows ce|xda|xiino/i.test(
            a
          ) ||
          /1207|6310|6590|3gso|4thp|50[1-6]i|770s|802s|a wa|abac|ac(er|oo|s\-)|ai(ko|rn)|al(av|ca|co)|amoi|an(ex|ny|yw)|aptu|ar(ch|go)|as(te|us)|attw|au(di|\-m|r |s )|avan|be(ck|ll|nq)|bi(lb|rd)|bl(ac|az)|br(e|v)w|bumb|bw\-(n|u)|c55\/|capi|ccwa|cdm\-|cell|chtm|cldc|cmd\-|co(mp|nd)|craw|da(it|ll|ng)|dbte|dc\-s|devi|dica|dmob|do(c|p)o|ds(12|\-d)|el(49|ai)|em(l2|ul)|er(ic|k0)|esl8|ez([4-7]0|os|wa|ze)|fetc|fly(\-|_)|g1 u|g560|gene|gf\-5|g\-mo|go(\.w|od)|gr(ad|un)|haie|hcit|hd\-(m|p|t)|hei\-|hi(pt|ta)|hp( i|ip)|hs\-c|ht(c(\-| |_|a|g|p|s|t)|tp)|hu(aw|tc)|i\-(20|go|ma)|i230|iac( |\-|\/)|ibro|idea|ig01|ikom|im1k|inno|ipaq|iris|ja(t|v)a|jbro|jemu|jigs|kddi|keji|kgt( |\/)|klon|kpt |kwc\-|kyo(c|k)|le(no|xi)|lg( g|\/(k|l|u)|50|54|\-[a-w])|libw|lynx|m1\-w|m3ga|m50\/|ma(te|ui|xo)|mc(01|21|ca)|m\-cr|me(rc|ri)|mi(o8|oa|ts)|mmef|mo(01|02|bi|de|do|t(\-| |o|v)|zz)|mt(50|p1|v )|mwbp|mywa|n10[0-2]|n20[2-3]|n30(0|2)|n50(0|2|5)|n7(0(0|1)|10)|ne((c|m)\-|on|tf|wf|wg|wt)|nok(6|i)|nzph|o2im|op(ti|wv)|oran|owg1|p800|pan(a|d|t)|pdxg|pg(13|\-([1-8]|c))|phil|pire|pl(ay|uc)|pn\-2|po(ck|rt|se)|prox|psio|pt\-g|qa\-a|qc(07|12|21|32|60|\-[2-7]|i\-)|qtek|r380|r600|raks|rim9|ro(ve|zo)|s55\/|sa(ge|ma|mm|ms|ny|va)|sc(01|h\-|oo|p\-)|sdk\/|se(c(\-|0|1)|47|mc|nd|ri)|sgh\-|shar|sie(\-|m)|sk\-0|sl(45|id)|sm(al|ar|b3|it|t5)|so(ft|ny)|sp(01|h\-|v\-|v )|sy(01|mb)|t2(18|50)|t6(00|10|18)|ta(gt|lk)|tcl\-|tdg\-|tel(i|m)|tim\-|t\-mo|to(pl|sh)|ts(70|m\-|m3|m5)|tx\-9|up(\.b|g1|si)|utst|v400|v750|veri|vi(rg|te)|vk(40|5[0-3]|\-v)|vm40|voda|vulc|vx(52|53|60|61|70|80|81|83|85|98)|w3c(\-| )|webc|whit|wi(g |nc|nw)|wmlb|wonu|x700|yas\-|your|zeto|zte\-/i.test(
            a.substr(0, 4)
          )
        )
          check = true;
      })(navigator.userAgent || navigator.vendor || window.opera);
      return check;
    },
  },
};
</script>
<style scoped>
.select-css {
  display: block;
  font-size: 16px;
  font-family: "Verdana", sans-serif;
  font-weight: 400;
  color: rgb(234, 235, 225);
  line-height: 1.3;
  padding: 0.4em 1.4em 0.3em 0.8em;
  width: 400px;
  max-width: 100%;
  box-sizing: border-box;
  margin: 20px auto;
  border: 1px solid #aaa;
  box-shadow: 0 1px 0 1px rgba(0, 0, 0, 0.03);
  border-radius: 0.3em;
  -moz-appearance: none;
  -webkit-appearance: none;
  appearance: none;
  background-color: rgb(17, 17, 17);
  background-image: url("data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%23007CB2%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-12.9%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2012.9l128%20127.9c3.6%203.6%207.8%205.4%2012.8%205.4s9.2-1.8%2012.8-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-12.8%200-5-1.9-9.2-5.5-12.8z%22%2F%3E%3C%2Fsvg%3E"),
    linear-gradient(to bottom, #0f0f0f 0%, #0c0c0c 100%);
  background-repeat: no-repeat, repeat;
  background-position: right 0.7em top 50%, 0 0;
  background-size: 0.65em auto, 100%;
}

.fondo {
  background-color: rgb(207, 204, 181);
  left: 100%;
  top: 100%;
  margin: 200;
}
</style>
