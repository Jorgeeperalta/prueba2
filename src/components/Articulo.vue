<template>
  <v-app>
    <v-card class="mx-auto" width="1200">
      <v-text-field color="purple darken-4" loading disabled></v-text-field>
      <template>
        <v-data-table
          :headers="headers"
          :items="desserts"
          sort-by="precio"
          class="elevation-1"
          :search="search"
        >
          <template v-slot:top>
            <v-toolbar flat color="black">
              <v-toolbar-title>Articulos</v-toolbar-title>

              <v-divider class="mx-4" inset vertical></v-divider>
              <v-spacer></v-spacer>
              <v-text-field
                v-model="search"
                label="Buscar (POR NOMBRE)"
                class="mx-2"
              ></v-text-field>

              <v-spacer></v-spacer>

              <v-dialog v-model="dialog" max-width="600px">
                <template v-slot:activator="{ on }">
                  <v-btn color="primary" dark class="mb-2" v-on="on"
                    >Nuevo Articulo</v-btn
                  >
                </template>

                <v-card>
                  <v-card-title>
                    <span class="headline">{{ formTitle }}</span>
                  </v-card-title>

                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col cols="6" sm="6" md="6">
                          <v-text-field
                            v-model="editedItem.id"
                            label="Id"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="6" sm="6" md="6">
                          <v-text-field
                            v-model="editedItem.precio"
                            label="precio"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="6" sm="6" md="6">
                          <select
                            v-model="editedItem.categoria"
                            class="select-css"
                          >
                            <option
                              v-for="(item, index) in items2"
                              :key="index"
                              v-bind:value="item.nombre"
                            >
                              {{ item.nombre }}
                            </option>
                          </select>
                        </v-col>
                        <v-col cols="6" sm="6" md="6">
                          <v-text-field
                            v-model="editedItem.nombre"
                            label="Nombre"
                          ></v-text-field>
                        </v-col>
                        <v-col cols="6" sm="6" md="6">
                          <template>
                            <v-text-field
                              v-model="editedItem.url"
                              label="URL"
                            ></v-text-field>
                          </template>
                        </v-col>
                        <v-col cols="6" sm="6" md="6">
                          <template>
                            <v-text-field
                              v-model="editedItem.url2"
                              label="URL"
                            ></v-text-field>
                          </template>
                        </v-col>
                        <v-col cols="6" sm="6" md="6">
                          <template>
                            <v-text-field
                              v-model="editedItem.url3"
                              label="URL"
                            ></v-text-field>
                          </template>
                        </v-col>
                      </v-row>
                    </v-container>
                  </v-card-text>

                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue darken-1" text @click="close"
                      >Cancelar</v-btn
                    >
                    <v-btn color="blue darken-1" text @click="save"
                      >Guardar</v-btn
                    >
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-toolbar>
          </template>
          <template v-slot:item.url="{ item }">
            <img
              @click="zoon(item.url, item.url2, item.url3)"
              width="60"
              height="60"
              v-bind:src="item.url"
            />
          </template>
          <template v-slot:item.url2="{ item }">
            <img
              @click="zoon(item.url, item.url2, item.url3)"
              width="60"
              height="60"
              v-bind:src="item.url2"
            />
          </template>
          <template v-slot:item.url3="{ item }">
            <img
              @click="zoon(item.url, item.url2, item.url3)"
              width="60"
              height="60"
              v-bind:src="item.url3"
            />
          </template>

          <template v-slot:item.precio="{ item }">
            <v-chip :color="getColor(item.precio)" dark>{{
              item.precio
            }}</v-chip>
          </template>
          <template v-slot:item.actions="{ item }">
            <v-icon small class="mr-2" @click="editItem(item)">
              view_comfy
            </v-icon>
            <v-icon small @click="deleteItem(item)">
              not_interested
            </v-icon>
          </template>
          <template v-slot:no-data>
            <v-btn color="primary" @click="initialize">Reset</v-btn>
          </template>
        </v-data-table>
      </template>

      <v-dialog v-model="dialog2" max-width="600px">
        <v-card>
          <v-carousel :show-arrows="false">
            <v-carousel-item :src="imag1"></v-carousel-item>
            <v-carousel-item :src="imag2"></v-carousel-item>
            <v-carousel-item :src="imag3"></v-carousel-item>
          </v-carousel>
          <v-btn block @click="dialog2 = false">Cerrar</v-btn>
        </v-card>
      </v-dialog>
    </v-card>
  </v-app>
</template>
<script>
export default {
  data: () => ({
    items2: [],
    dialog2: false,
    imag1: "",
    imag2: "",
    imag3: "",
    dialog: false,
    search: "",
    idd: "",
    categoriaa: "",
    nombree: "",
    precioo: "",
    urll: "",
    urll2: "",
    urll3: "",
    headers: [
      {
        text: "Identificador",
        align: "start",
        sortable: false,
        value: "id",
      },

      { text: "categoria", value: "categoria" },
      { text: "Nombre", value: "nombre" },

      { text: "Precio", value: "precio" },
      { text: "Imagen 1", value: "url" },
      { text: "Imagen 2", value: "url2" },
      { text: "Imagen 3", value: "url3" },
      { text: "Actions", value: "actions", sortable: false },
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      id: "",
      precio: 0,
      categoria: 0,
      nombre: 0,
      url: "",
      url2: "",
      url3: "",
    },
    defaultItem: {
      id: "",
      precio: 0,
      categoria: 0,
      nombre: 0,
      url: "",
      url2: "",
      url3: "",
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "Nuevo Articulo" : "Edita Articulo";
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
  },

  created() {
    this.autocompleta();
    this.initialize();
  },

  methods: {
    zoon(ima1, ima2, ima3) {
      this.imag1 = ima1;
      this.imag2 = ima2;
      this.imag3 = ima3;
      this.dialog2 = true;
    },
    cargar_datos() {
      var cont = 0;
      async function asyncData() {
        const response = await fetch(
          "http://jorgeperalta-001-site6.itempurl.com/articulos.php"
        );
        const data = await response.json();

        return data;
      }

      const result = asyncData();

      result.then((data) => {
        this.desserts = data;
        console.log(data);
      });
    },
    autocompleta() {
      var cont = 0;
      async function asyncData() {
        const response = await fetch(
          "http://jorgeperalta-001-site6.itempurl.com/catalogo.php"
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

    getColor(precio) {
      if (precio > 400) return "purple darken-3";
      else if (precio > 200) return "teal darken-2";
      else return "blue darken-2";
    },
    initialize() {
      this.cargar_datos();
    },

    editItem(item) {
      this.editedIndex = this.desserts.indexOf(item);

      this.editedItem = Object.assign({}, item);
      this.dialog = true;
      this.idd = item.id;
      this.categoriaa = item.categoria;
      this.nombree = item.nombre;
      this.precioo = item.precio;
      this.urll = item.url;
      this.urll2 = item.url2;
      this.urll3 = item.url3;
      this.cargar_datos();
    },

    close() {
      this.dialog = false;
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      }, 300);
    },

    save() {
      if (this.editedIndex > -1) {
        var requestOptions = {
          method: "PUT",
          redirect: "follow",
        };

        fetch(
          "http://jorgeperalta-001-site6.itempurl.com/articulos.php?id=" +
            this.editedItem.id +
            "&categoria=" +
            this.editedItem.categoria +
            "&nombre=" +
            this.editedItem.nombre +
            "&precio=" +
            this.editedItem.precio +
            "&url=" +
            this.editedItem.url +
            "&url2=" +
            this.editedItem.url2 +
            "&url3=" +
            this.editedItem.url3,
          requestOptions
        )
          .then((response) => response.text())
          .then((result) => console.log(result));

        this.cargar_datos();
      } else {
        var formdata = new FormData();

        formdata.append("id", this.editedItem.id);
        formdata.append("categoria", this.editedItem.categoria);
        formdata.append("nombre", this.editedItem.nombre);
        formdata.append("precio", this.editedItem.precio);
        formdata.append("url", this.editedItem.url);
        formdata.append("url2", this.editedItem.url2);
        formdata.append("url3", this.editedItem.url3);
        var requestOptions = {
          method: "POST",
          body: formdata,
          redirect: "follow",
        };

        fetch(
          "http://jorgeperalta-001-site6.itempurl.com/articulos.php",
          requestOptions
        )
          .then((response) => response.text())
          .then((result) => console.log(result));

        this.cargar_datos();

        this.close();
      }
      this.close();
    },
    deleteItem(item) {
      const index = this.desserts.indexOf(item);

      confirm("Are you sure you want to delete this item?") &&
        this.desserts.splice(index, 1);

      var requestOptions = {
        method: "DELETE",

        redirect: "follow",
      };

      fetch(
        "http://jorgeperalta-001-site6.itempurl.com/articulos.php?id=" +
          item.id,
        requestOptions
      )
        .then((response) => response.text())
        .then((result) => console.log(result))
        .catch((error) => console.log("error", error));

      this.cargar_datos();
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
</style>
