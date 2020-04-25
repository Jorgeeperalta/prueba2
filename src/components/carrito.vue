<template>
  <div class="tex" >
    
    <h1>Carrito de compra</h1>
    
   

    <br>
    <div  v-for="(item,index) in items" :key="index">
       
      <div >
        <div>{{ item.nombre }} Cant. {{ item.qty }}</div>
         <div>{{ item.precio }}</div>
       
      </div>
      <div >Sub-Total $ {{ item.qty * item.precio }}</div>
      <div>------------------</div>
    </div>
---------------------------------------------------------------------------------
    <div >
      Total de su
      compra 
      $ {{ total }}
 <!--      <h1>{{items}}</h1>  -->
    </div>
  </div>
 
</template>
<script>
import logica from "../logica";
import _ from "lodash";

export default {
  data() {
    return {
      items: logica.data.cart,
      
    };
   
  },
  computed: {
    total() {
      return _.sumBy(this.items, function (it) {
        return it.precio * it.qty;
      });
    
    },
  },
};
</script>
<style>
img {
  width: 80%;
}
.imgsd {
  width: 55%;
}
.car_pro {
  display: flex;
  justify-content: space-between;
  padding: 0.8em;
  background-color: #fff;
  box-shadow: 0 0 10px gray;
  margin-top: 0.4em;
}
.todo h1 {
  text-align: center;
  background-color: #2c23aa;
  padding: 0.5em;
  margin: 0;
  color: white;
}
.total {
  border: 1px solid black;
  padding: 0.5em;
}
.tex{
  background-color: rgb(243, 148, 6);
}
</style>
