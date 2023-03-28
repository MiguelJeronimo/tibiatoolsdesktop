<script>
import blessings_vista from "../components/blessings.vue";
import experiencia_compartida from "../components/experiencia_compartida.vue";
import personajes_tibia from "../components/personajes.vue"
import stamina_tibia from '../components/stamina.vue'
import lista_mundos from "../components/mundos.vue";
import informacion_mundos from "../components/informacion_mundos.vue"
export default {
    name: 'menu_inicio',
    data(){
      return {
        menus: 'blesings_vista',
        rashid:''
      }
    }, 
    components:{
      blessings_vista,
      experiencia_compartida,
      personajes_tibia,
      stamina_tibia,
      lista_mundos,
      informacion_mundos
    }, methods:{
      PlayersOnline(){
        Rashid()
        .then((data)=>{
            this.rashid = `Hoy Rashid se encuentra en ${data}`
          }
        )
      }
    }, 
    mounted: ()=>{
      //cuando recarge la pagina mandamos a llamar la funcion playeronline y que se este ejecutando cada 5 segundos
    setInterval(() => {
        playerOnline()
      }, 5000)
    } 
}

function playerOnline() {
    let url = `https://api.tibiadata.com/v3/worlds`;
    let personaje_online = document.getElementById('personaje_online');
    let onlines;
    fetch(url)
    .then((resp) => resp.json())
    .then(function (data) {
       try {
        onlines = data.worlds.players_online;
        personaje_online.innerText= 'Personajes online: '+onlines;
       } catch (error) {
           console.log(error);
       }
    }).catch(function (error) {
        console.log(error.error());
    });
  }

  async function Rashid() {
    let url = `https://api.tibialabs.com/v1/rashid/city`;
    let rashid;
    rashid = fetch(url)
    let respuesta = (await rashid).text()
    return respuesta
  }
</script>

<template>
<!-- Always shows a header, even in smaller screens. -->
{{PlayersOnline()}}
<div class="mdl-layout mdl-js-layout mdl-layout--fixed-header">
    <header class=" barraOpciones mdl-layout__header">
      <div class="mdl-layout__header-row">
        <!-- Title -->
        <span class="mdl-layout-title">Ttools</span>
        <!-- Add spacer, to align navigation to the right -->
        <div class="mdl-layout-spacer"></div>
        <!-- Navigation. We hide it in small screens. -->
        <nav class="mdl-navigation online">
          <spam class="mdl-navigation__link online" id="rashid"><img src="../assets/img/rashid.gif">{{rashid}}</spam>
          <spam id="personaje_online"></spam>
        </nav>
      </div>
    </header>
    <div class="mdl-layout__drawer barraOpciones">
      <span class="mdl-layout-title">Ttools</span>
      <nav class="mdl-navigation">
        <a class="mdl-navigation__link"  @click=" menus = 'blesings_vista'">Blessings</a>
        <a class="mdl-navigation__link"  @click="menus='experiencia_compartida'">Experiencia Compartida</a>
        <a class="mdl-navigation__link"  @click="menus='personajes'">Personajes</a>
        <a class="mdl-navigation__link" @click="menus = 'stamina'">Stamina</a>
        <a class="mdl-navigation__link" @click="menus = 'lista_mundos'">Mundos</a>
        <a class="mdl-navigation__link" @click="menus = 'informacion_mundos'">Información de Mundos</a>
        <a class="mdl-navigation__link" >Criaturas</a>
      </nav>
    </div>
</div>
<blessings_vista v-show="menus === 'blesings_vista'"></blessings_vista>
<experiencia_compartida v-show="menus === 'experiencia_compartida'"></experiencia_compartida>
<personajes_tibia v-show="menus === 'personajes'"></personajes_tibia>
<stamina_tibia v-show="menus==='stamina'"></stamina_tibia>
<lista_mundos v-show="menus==='lista_mundos'"></lista_mundos>
<informacion_mundos v-show="menus ==='informacion_mundos'"></informacion_mundos>
</template>

<style>
    @import '../assets/css/material.css';
    @import '../assets/css/scroll.css';
    @import '../assets/css/blessings.css';
</style>