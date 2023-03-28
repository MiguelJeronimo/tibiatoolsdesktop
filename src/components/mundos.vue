<script>
    export default {
        name: 'lista_mundos',
        data(){
            return{
                mundos: null
            }
        },
        mounted() {
            setInterval(() => {
                obtenerMundos().then((data)=>{
                this.mundos = data.worlds.regular_worlds
            })
            }, 5000);
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
  <div class="titulo"><h4>Mundos</h4></div>
  <div class="titulo">

</div>

<div class="tabla" id="tabla"> 
<table class="mdl-data-table mdl-js-data-table with=1000px">
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
    @import '../assets/css/character.css';
</style>