<template class="home">
  <v-card class="black">
    <div v-if="bandera != 1">
      <v-app-bar
        absolute
        color="purple accent-4"
        dark
        shrink-on-scroll
        prominent
        src="https://picsum.photos/1920/1080?random"
        fade-img-on-scroll
        scroll-target="#scrolling-techniques-5"
        scroll-threshold="250"
      >
        <template v-slot:img="{ props }">
          <v-img
            v-bind="props"
            gradient="to top right, rgba(55,236,186,.7), rgba(25,32,72,.7)"
          ></v-img>
        </template>
        <v-btn icon @click="cambiar()">
          <v-icon>android</v-icon>
        </v-btn>
        <v-toolbar-title>Delivery </v-toolbar-title>

        <v-spacer></v-spacer>
      </v-app-bar>
      <v-sheet id="scrolling-techniques-5" class="overflow-y-auto">
        <br /><br /><br /><br /><br />
        <HelloWorld msg="Bienvenido a mi App Web" />

        <v-container class="black" fluid style="height: 450px">
          <v-card
            color="black"
            class="mx-auto"
            max-width="1000"
            :elevation="25"
            outlined
          >
            <v-spacer></v-spacer><br />
            <v-img
              src="https://picsum.photos/1920/1080?random"
              height="420"
              width="1200"
            ></v-img>

            <v-container class="pa-4 text-center">
              <v-row class="fill-height" align="center" justify="center">
                <template v-for="(item, i) in items">
                  <v-col :key="i" cols="12" md="4">
                    <v-hover v-slot:default="{ hover }">
                      <v-card
                        :elevation="hover ? 12 : 2"
                        :class="{ 'on-hover': hover }"
                      >
                        <v-img :src="item.img" height="225px">
                          <v-card-title class="title white--text">
                            <v-row
                              class="fill-height flex-column"
                              justify="space-between"
                            >
                              <p class="mt-4 subheading text-left">
                                {{ item.title }}
                              </p>

                              <div>
                                <p
                                  class="ma-0 body-1 font-weight-bold font-italic text-left"
                                >
                                  {{ item.text }}
                                </p>
                                <p
                                  class="caption font-weight-medium font-italic text-left"
                                >
                                  {{ item.subtext }}
                                </p>
                              </div>
                            </v-row>
                          </v-card-title>
                        </v-img>
                      </v-card>
                    </v-hover>
                  </v-col>
                </template>
              </v-row>
            </v-container>
            <v-dialog v-model="drawer">
              <template>
                <v-card
                  class="mx-auto"
                  height="400"
                  width="256"
                  color="blue-grey darken-3"
                >
                  <v-img
                    class="white--text align-end"
                    height="200px"
                    width="256"
                    src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
                  >
                  </v-img>
                  <v-card-subtitle class="pb-0">
                    <v-text-field
                      v-model="email"
                      autofocus
                      color="purple lighten-2"
                      label="Email"
                      required
                      background-color="black"
                    >
                    </v-text-field>
                    <v-text-field
                      v-model="password"
                      type="password"
                      color="purple lighten-2"
                      label="Password"
                      required
                      background-color="black"
                    >
                    </v-text-field>

                    <v-btn block @click="control()" color="purple darken-4"
                      >Ingresar</v-btn
                    >
                  </v-card-subtitle>

                  <template v-slot:append>
                    <div class="pa-2">
                      <v-btn block @click="drawer = false">Logout</v-btn>
                    </div>
                  </template>
                </v-card>
              </template>
            </v-dialog>
            <template>
              <v-card color="black">
                <h1>dfd</h1>
                <v-container class="home">
                  <v-row justify="space-between" class="home">
                    <v-col cols="auto" class="home">
                      <h3>
                        Cards The v-card component is a versatile component that
                        can be used for anything from a panel to a static image.
                        The card component has numerous helper components to
                        make markup as easy as possible. Components that have no
                        listed options use Vue's functional component option for
                        faster rendering and serve as markup sugar to make
                        building easier.
                      </h3>
                    </v-col>

                    <v-col cols="auto" class="text-center pl-0"> </v-col>
                  </v-row>
                </v-container>
              </v-card>
            </template>

            <br /><br /><br /><br />
          </v-card>
        </v-container>
      </v-sheet>
      <template> </template>
    </div>
    <div v-if="bandera == 1">
      <template v-if="op==0">
        <v-card
          class="mx-auto"
          height="400"
          width="256"
          color="blue-grey darken-3"
        >
          <v-img
            class="white--text align-end"
            height="200px"
            width="256"
            src="https://cdn.vuetifyjs.com/images/cards/docks.jpg"
          >
          </v-img>
          <v-card-subtitle class="pb-0">
            <v-btn block @click="opcion(1)" color="purple darken-4"
              >Catalogo</v-btn
            >
          </v-card-subtitle>

          <v-card-subtitle class="pb-0">
            <v-btn block @click="opcion(2)" color="purple darken-4"
              >Articulos</v-btn
            >
          </v-card-subtitle>
        </v-card>
      </template>
          <v-btn block @click="opcion(0)" v-if="op!=0" color="purple darken-4"
              >Menu</v-btn
            >
      <div v-if="op == 1">
        <catalogo></catalogo>
      </div>
      <div v-if="op == 2">
        <articulo></articulo>
      </div>
    </div>
  </v-card>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import articulo from "@/components/Articulo.vue";
import catalogo from "@/components/Catalogo.vue";
export default {
  name: "Home",
  components: {
    HelloWorld,
    articulo,
    catalogo,
  },
  data() {
    return {
      drawer: false,
      email: "",
      password: "",
      error: null,
      bandera: 0,
      op: 10,
      datos: [],
      items2: [
        { title: "Dashboard", icon: "dashboard" },
        { title: "Account", icon: "account_box" },
        { title: "Admin", icon: "gavel" },
      ],
      items: [
        {
          title: "New Releases",
          text: "It's New Release Friday",
          subtext: "Newly released songs. Updated daily.",
          img: "http://lorempixel.com/output/nightlife-q-c-640-480-5.jpg",
        },
        {
          title: "Rock",
          text: "Greatest Rock Hits",
          subtext: "Lose yourself in rock tunes.",
          img:
            "https://images.unsplash.com/photo-1498038432885-c6f3f1b912ee?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2100&q=80",
        },
        {
          title: "Mellow Moods",
          text: "Ambient Bass",
          subtext: "Chill beats to mellow you out.",
          img: "http://lorempixel.com/output/abstract-q-c-640-480-6.jpg",
        },
      ],
      items1: [
        {
          src:
            "https://cdn.pixabay.com/photo/2015/10/01/05/04/flowershop-966459_960_720.jpg",
        },
        {
          src:
            "https://cdn.pixabay.com/photo/2017/06/21/20/51/tshirt-2428521__340.jpg",
        },
        {
          src:
            "https://cdn.pixabay.com/photo/2016/03/02/20/13/shopping-1232944__340.jpg",
        },
        {
          src:
            "https://cdn.pixabay.com/photo/2015/11/07/11/00/meat-1030729__340.jpg",
        },
      ],
    };
  },
  created() {
    this.ingresar();
  },
  methods: {
    opcion(num) {
      this.op = num;
    },
    ingresar() {
      var cont = 0;
      async function asyncData() {
        const response = await fetch(
          "http://jorgeperalta-001-site6.itempurl.com/login.php"
        );
        const data = await response.json();

        return data;
      }

      const result = asyncData();

      result.then((data) => {
        data.forEach((element) => {
          this.datos[cont] = element;
          cont++;
        });
      });
    },
    control() {
      this.datos.forEach((element) => {
        if (element.email == this.email && element.password == this.password) {
          if (element.estato == 0) {
            this.bandera = 0;
          } else if (element.estato == 1) {
            this.bandera = 1;
          
          } else {
            alert("No puede Ingresar");
            this.bandera = 2;
          }
        }
      });

      this.$emit("estado_login", this.bandera);
    },

    cambiar() {
      this.drawer = true;
    },
  },
};
</script>
<style>
.home {
  background-color: black;
}
.v-card {
  transition: opacity 0.9s ease-in-out;
}

.v-card:not(.on-hover) {
  opacity: 1;
}

.show-btns {
  color: rgb(221, 42, 42) !important;
}
</style>
