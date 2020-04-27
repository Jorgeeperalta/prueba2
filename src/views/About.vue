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

      <v-card class="mx-auto" height="400" width="256" color="orange">
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
      </v-card>
    </v-dialog>
    <template>
      <v-row>
        <v-dialog v-model="dialog" width="500px">
          <!-- carrito de compra -->
          <v-card dark color="black">
            <v-card-title>
              <span class="headline"></span>
            </v-card-title>
            <carrito></carrito>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-flex xs10>
                <v-text-field
                  single-line
                  outline
                  color="teal"
                  v-model="nombre"
                  label="Nombre"
                ></v-text-field>
              </v-flex>
              <br />
              <br />
              <v-flex xs10>
                <v-text-field
                  label="Direccion"
                  single-line
                  color="teal"
                  class="my-input"
                  v-model="text"
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
                    dialog = false;

                    abrir = true;
                  "
                  :href="apilink"
                  target="_blank"
                >
                  <v-icon large>send</v-icon>
                </v-btn>
              </v-flex>
              <div><form ref="myform"></form></div>
              <v-spacer></v-spacer>

              <br />

              <v-btn color="red" @click="dialog = false"
                ><v-icon>delete_forever</v-icon></v-btn
              >
            </v-card-actions>
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

    <!--   <v-dialog v-model="abrir">

            <h1>Puede pagar cuando llega el producto o pagar con mercadopago</h1>
                         <div>  <form ref="myform">
      
                       </form> </div>
          </v-dialog>   -->
  </v-app>
</template>

<script>
import producto from "@/components/producto.vue";
import carrito from "@/components/carrito.vue";
import logica from "../logica";
import _ from "lodash";

export default {
  components: {
    producto,
    carrito,
  },

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

      dialog: false,
      drawer: null,

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
      total_pruductos: [
        /* total
        {
          id: 1,
            categoria:1,
          nombre: "Lavandina",
        
          precio: 885,
          image:[
            
             {src: "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg"} ,
             { src: "https://cdn.pixabay.com/photo/2016/04/19/13/39/store-1338629__340.jpg"},
             { src:"https://cdn.pixabay.com/photo/2018/01/25/08/14/tangerines-3105628__340.jpg"},
            
          
          ] ,
       },
       */
        // anicio
        {
          id: 2,
          categoria: 1,
          nombre: "Zanahoria",
          precio: 42,
          image: [
            {
              src:
                "https://cdn.pixabay.com/photo/2016/11/30/12/29/bicycle-1872682__340.jpg",
            },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/01/25/08/14/tangerines-3105628__340.jpg",
            },
          ],
        },
        // fin
        // anicio
        {
          id: 3,
          categoria: 1,
          nombre: "Telefono",
          precio: 65,
          image: [
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/01/25/08/14/tangerines-3105628__340.jpg",
            },
          ],
        },
        // fin
        // anicio
        {
          id: 4,
          categoria: 2,
          nombre: "Jabon en polvo",
          precio: 85,
          image: [
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/01/25/08/14/tangerines-3105628__340.jpg",
            },
          ],
        },
        // fin
        // anicio
        {
          id: 5,
          categoria: 2,
          nombre: "Cerveza",
          precio: 75,
          image: [
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/01/25/08/14/tangerines-3105628__340.jpg",
            },
          ],
        },
        // fin
        // anicio
        {
          id: 6,
          categoria: 2,
          nombre: "Coca Cola",
          precio: 45,
          image: [
            {
              src:
                "https://cdn.pixabay.com/photo/2012/07/29/21/42/dresses-53319__340.jpg",
            },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/01/25/08/14/tangerines-3105628__340.jpg",
            },
          ],
        },
        // fin
        // anicio
        {
          id: 7,
          categoria: 3,
          nombre: "Vino",
          precio: 99,
          image: [
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            { src: "https://i.ibb.co/HDj6ZnH/chorizos-listos-parrilla.png" },
          ],
        },
        // fin
        // anicio
        {
          id: 8,
          categoria: 3,
          nombre: "Bananas",
          precio: 31,
          image: [
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            { src: "https://i.ibb.co/Lx3N6L8/LANCHE.jpg" },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/01/25/08/14/tangerines-3105628__340.jpg",
            },
          ],
        },
        // fin
        // anicio
        {
          id: 9,
          categoria: 3,
          nombre: "Verdurita",
          precio: 5,
          image: [
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            {
              src:
                "https://cdn.pixabay.com/photo/2018/05/24/02/07/peach-3425656__340.jpg",
            },
            { src: "https://i.ibb.co/HhcwrsJ/pata.jpg" },
          ],
        },
        // fin
      ],
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
    cargar_datos() {
      fetch("http://localhost:81/carro2/src/backend/post.php")
        .then((datos) => datos.json())
        .then((datos) => {
          //  console.log(datos);
          this.total_productos2 = datos;
        });

      fetch("http://localhost:81/carro2/src/backend/post_imag.php")
        .then((datos) => datos.json())
        .then((datos) => {
          this.total_productos2.imagenes = datos;
        });
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
      for (var i = 0; i < this.total_productos2.length; i++) {
        // alert(this.total_productos2[i].nombre);
        if (valor == this.total_productos2[i].categoria) {
          //  alert(this.total_productos2[i].nombre);
          this.productos.push({
            id: this.total_productos2[i].id,
            categoria: this.total_productos2[i].categoria,
            nombre: this.total_productos2[i].nombre,
            precio: this.total_productos2[i].precio,
           
            image: [
             
                { src:this.total_productos2.imagenes[i].src,},
            
                 {src:this.total_productos2.imagenes[i+1].src,},
           
               {src:this.total_productos2.imagenes[i+2].src,}],

          });
          cont++;
        }
      }
     

      console.log(this.productos);
      /*   (this.productos = []),
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
.fondo {
  background-color: rgb(207, 204, 181);
  left: 100%;
  top: 100%;
  margin: 200;
}
</style>
