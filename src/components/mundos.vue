<script>
    export default {
        name: 'lista_mundos',
        data(){
            return{
                mundos: null,
                spinner_loadin: true,
                table_mundos: false
            }
        },
        mounted() {
            //agregando la intermitencia a la tabla para imprimir los datos en tiempo real
            let Mundos = setInterval(() => {
                obtenerMundos().then((data)=>{
                    this.mundos = data.worlds.regular_worlds
                    //apareciando la tabla una vez mostrado los datos
                    this.table_mundos = true
                    //ocultar el spinner
                    this.spinner_loadin = false
                })
            }, 2000);
            //dandole tiempo de 3 segundos para que puedar ejecutar el spinner
            //y despues eliminarlo
            setTimeout(() => {
                Mundos
            }, 3000);
        }
    }
async function obtenerMundos(){
    let url = 'https://api.tibiadata.com/v3/worlds';
    let respuesta = fetch(url)
    let mundos = (await respuesta).json();
    return mundos
 }
</script>

<template>
<main class="mdl-layout__content barraOpciones tablas contenedor">

<div class="page-content">
    <br><br><br>
  <div class="titulo"><h4>Mundos</h4></div>
  <div class="titulo">
</div>
<div class="tabla" id="tabla">
<div class="spinner" style="text-align: center;" v-if="spinner_loadin == true"></div>
<table class="mdl-data-table mdl-js-data-table with=1000px" v-if="table_mundos==true">
  <thead>
    <tr>
      <th class="mdl-data-table__cell--non-numeric">Mundo</th>
      <th>Estado</th>
      <th>Localización</th>
      <th class = "mdl-data-table__cell--non-numeric">Tipo PVP</th>
      <th class = "mdl-data-table__cell--non-numeric">Premium Only</th>
      <th class = "mdl-data-table__cell--non-numeric">Tranfer type</th>
      <th class = "mdl-data-table__cell--non-numeric">Players online</th>
    </tr>
  </thead>
  <tbody id="tablas" class = "">
    <tr v-for="mundo in mundos" :key="mundo">
        <td class="mdl-data-table__cell--non-numeric">{{mundo.name}}</td>
        <td class="mdl-data-table__cell--non-numeric">{{mundo.status}}</td>
        <td class="mdl-data-table__cell--non-numeric">{{mundo.location}}</td>
        <td class="mdl-data-table__cell--non-numeric">{{mundo.pvp_type}}</td>
        <td class="mdl-data-table__cell--non-numeric">{{mundo.premium_only}}</td>
        <td class="mdl-data-table__cell--non-numeric">{{mundo.transfer_type}}</td>
        <td class="mdl-data-table__cell--non-numeric online">{{mundo.players_online}}</td>
    </tr>
  </tbody>
  </table>
</div>
</div>
</main>
</template>
<style scoped>
    @import '../assets/css/spinner.css';
    @import '../assets/css/character.css';
</style>