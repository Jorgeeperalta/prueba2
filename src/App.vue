<template>
  <v-app>
    <div id="app">
      <div id="nav">
        <router-link to="/">Pagina principal</router-link> |
        <router-link to="/about_2">Tienda</router-link>
      </div>
      <router-view />
    </div>
  </v-app>
</template>

<style>
#app {
  background-color: black;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: red;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color:orange;
  
}

#nav a.router-link-exact-active {
  color:green;
}
</style>
