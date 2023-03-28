<script>
export default {
    name: 'informacion_mundos',
    data(){
        return {
            select_mundos:'',
            mundo:'-',
            players:'-',
            Record:'-',
            creation_date:'-',
            Location:'-',
            type:'-',
            battlEye:'-',
            world_quest:'',
            players_online:'',
            spinner_loadin: false,
            tablas: false
        }
    },methods:{
        Select(world){
            if (world != '') {
              this.spinner_loadin= true
              this.tablas = false
              this.mundo = '-'
              this.players = '-'
              this.Record = '-'
              this.creation_date = '-'
              this.Location = '-'
              this.type = '-'
              this.battlEye = '-'
              this.world_quest = ''
              this.players_online = ''
              let Informacion = InfoWorlds(world).then((data)=>{
                  this.mundo = data.worlds.world.name;
                  this.players = data.worlds.world.players_online;
                  this.Record = data.worlds.world.record_players +' Players el: '+data.worlds.world.record_date;
                  this.creation_date = data.worlds.world.creation_date;
                  this.Location = data.worlds.world.location;
                  this.type = data.worlds.world.pvp_type;
                  this.battlEye = data.worlds.world.battleye_protected;
                  this.world_quest = data.worlds.world.world_quest_titles
                  this.players_online = data.worlds.world.online_players
                  this.spinner_loadin = false,
                  this.tablas = true
                })
                setTimeout(() => {
                  Informacion
                }, 2000);
            } else{
                    this.mundo = '-'
                    this.players = '-'
                    this.Record = '-'
                    this.creation_date = '-'
                    this.Location = '-'
                    this.type = '-'
                    this.battlEye = '-'
                    this.world_quest = ''
                    this.players_online = ''
                    this.spinner_loadin = false
                    this.tablas = false
            }
        }
    },
    mounted(){
        obtenerMundos().then((data)=>{
            this.select_mundos = data.worlds.regular_worlds
        })
        
    }
}


async function obtenerMundos() {
    let url = 'https://api.tibiadata.com/v3/worlds';
    let respuesta = fetch(url)
    let worlds = (await respuesta).json()
    return worlds
}

async function InfoWorlds(world) {
    let url = `https://api.tibiadata.com/v3/world/${world}`;
    let respuesta = fetch(url)
    let dataWorld = (await respuesta).json()
    return dataWorld;
}
</script>
<template>
    <main class="mdl-layout__content barraOpciones tablas contenedor">

<div class="page-content"><!-- Your content goes here -->
  <div class="titulo"><h4>Informacion de Mundos</h4></div>
  <div class="titulo">
          <div class="separador">
              <div class="mdl-menu__container is-visible " id="formulario">
                      <select class="mdl-menu__item"  name="" id="selectWorlds" @change="event => Select(event.target.value)">
                        <option value="">Seleccione una opción</option>
                        <option v-for="select in select_mundos" :key="select" :value="select.name">{{select.name}}
                        </option>
                      </select>
              </div>
          </div>
          <br>
          <div  class="mdl-grid titulo tamaño">
              <div class="mdl-cell mdl-cell--6-col-phone"><Strong>Mundo:</Strong></div>
              <div class="mdl-cell mdl-cell--2-col-phone" id="mundo">{{mundo}}</div>
      
              <div class="mdl-cell mdl-cell--6-col-phone"><Strong>Players Online:</Strong></div>
              <div class="mdl-cell mdl-cell--2-col-phone" id="players">{{players}}</div>
              
              <div class="mdl-cell mdl-cell--6-col-phone"><strong>Online Record:</strong></div>
              <div class="mdl-cell mdl-cell--2-col-phone" id = "record_online">{{Record}}</div>
      
              <div class="mdl-cell mdl-cell--6-col-phone"><strong>Creation Date:</strong></div>
              <div class="mdl-cell mdl-cell--2-col-phone" id="fecha_creacion">{{creation_date}}</div>
      
              <div class="mdl-cell mdl-cell--6-col-phone"><strong>Location:</strong></div>
              <div class="mdl-cell mdl-cell--2-col-phone" id="localizacion">{{Location}}</div>

              <div class="mdl-cell mdl-cell--6-col-phone"><strong>PvP Type:</strong></div>
              <div class="mdl-cell mdl-cell--2-col-phone" id="tipo_pvp">{{type }}</div>

              <div class="mdl-cell mdl-cell--6-col-phone"><strong>BattlEye Status:</strong></div>
              <div class="mdl-cell mdl-cell--2-col-phone" id="battleeye">{{battlEye}}</div>
          </div>
  </div>
</div>
<div class="worldQuest">
  <h5 v-if="tablas == true">Quest World Title</h5>
  <div class="spinner" style="text-align: center;" v-if="spinner_loadin == true"></div>
</div>
<div class="worldQuest" v-if="tablas == true">
  <ul class="demo-list-icon mdl-list" id="worldQuest">
    <li class="mdl-list__item" v-for="world in world_quest" :key="world">
        <span class="mdl-list__item-primary-content">
            <i class="material-icons mdl-list__item-icon">star</i>
                {{world}}
        </span>
    </li>
  </ul>
</div>

<div class="tabla" id="tabla" v-if="tablas == true"> 
<table class="mdl-data-table mdl-js-data-table with=1000px">
  <thead>
    <tr>
      <th class="mdl-data-table__cell--non-numeric">Nombre</th>
      <th class="mdl-data-table__cell--non-numeric">Level</th>
      <th class="mdl-data-table__cell--non-numeric">Vocation</th>
    
    </tr>
  </thead>
  <tbody id="datos">
    <tr v-for="playersOnline in players_online" :key="playersOnline">
        <td class="mdl-data-table__cell--non-numeric" id="nombre">{{playersOnline.name}}</td>
        <td class="mdl-data-table__cell--non-numeric" id="level">{{playersOnline.level}}</td>
        <td class="mdl-data-table__cell--non-numeric" id="vocacion">{{playersOnline.vocation}}</td> 
    </tr>
  </tbody>
  </table>
</div>
</main>
</template>

<style scoped>
@import '../assets/css/informacion_mundos.css';
</style>