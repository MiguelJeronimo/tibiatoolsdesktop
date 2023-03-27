<script>
import blessings_vista from "../components/blessings.vue";
import experiencia_compartida from "../components/experiencia_compartida.vue";
import personajes_tibia from "../components/personajes.vue"
import stamina_tibia from '../components/stamina.vue'
export default {
    name: 'menu_inicio',
    data(){
      return {
        menus: 'blesings_vista'
      }
    }, 
    components:{
      blessings_vista,
      experiencia_compartida,
      personajes_tibia,
      stamina_tibia
    }
}
</script>

<template>
<!-- Always shows a header, even in smaller screens. -->

<div class="mdl-layout mdl-js-layout mdl-layout--fixed-header">
    <header class=" barraOpciones mdl-layout__header">
      <div class="mdl-layout__header-row">
        <!-- Title -->
        <span class="mdl-layout-title">Ttools</span>
        <!-- Add spacer, to align navigation to the right -->
        <div class="mdl-layout-spacer"></div>
        <!-- Navigation. We hide it in small screens. -->
        <nav class="mdl-navigation online">
          <spam class="mdl-navigation__link online" id="rashid"></spam>
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
        <a class="mdl-navigation__link" >Mundos</a>
        <a class="mdl-navigation__link" >Información de Mundos</a>
        <a class="mdl-navigation__link" >Criaturas</a>
      </nav>
    </div>
</div>
<blessings_vista v-show="menus === 'blesings_vista'"></blessings_vista>
<experiencia_compartida v-show="menus === 'experiencia_compartida'"></experiencia_compartida>
<personajes_tibia v-show="menus === 'personajes'"></personajes_tibia>
<stamina_tibia v-show="menus==='stamina'"></stamina_tibia>
</template>

<style>
    @import '../assets/css/material.css';
    @import '../assets/css/scroll.css';
    @import '../assets/css/blessings.css';
</style>