<script>
export default {
    name:'criaturas_tibia',
    data(){
        return {
            boosData:'',
            criaturas:'',
            spinner_loadin: true,
            table_creature: false
        }
    }, mounted(){

        let Criaturas = obtenerCriaturas().then((data)=>{
            this.boosData = data.creatures.boosted
            this.criaturas = data.creatures.creature_list
            this.table_creature = true
            this.spinner_loadin = false
          })

        setTimeout(() => {
            Criaturas
        }, 5000);
    }
}

async function obtenerCriaturas(){
    let url = 'https://api.tibiadata.com/v3/creatures';
    let respuesta = fetch(url)
    let criaturas = (await respuesta).json()
    return criaturas
}


</script>
<template>
<main class="mdl-layout__content barraOpciones tablas contenedor">
<div class="page-content"><!-- Your content goes here -->
 <br><br>
   <div class="titulo"><h4>Criaturas</h4>
   <div class="tabla" id="tabla">
    <div class="spinner" style="text-align: center;" v-if="spinner_loadin == true"></div>
    <table class="mdl-data-table mdl-js-data-table with=1000px" v-if="table_creature==true">
      <tbody id="tablas" class = "">
        <tr>
             <td class="mdl-data-table__cell--non-numeric"><img :src="boosData.image_url" alt=""></td>
             <td class="mdl-data-table__cell--non-numeric">Today´s boss: {{boosData.name}}</td>
             <td class="mdl-data-table__cell--non-numeric">{{boosData.race}}</td>
        </tr>
        <tr v-for="criatura in criaturas" :key="criatura">
            <td class="mdl-data-table__cell--non-numeric"><img :src="criatura.image_url"></td>
            <td class="mdl-data-table__cell--non-numeric">{{criatura.name}}</td>
            <td class="mdl-data-table__cell--non-numeric">{{criatura.race}}</td>
        </tr>
      </tbody>
      </table>
   </div>
   </div>
</div>
  </main>
</template>
<style scoped>
    @import '../assets/css/spinner.css';
    @import '../assets/css/character.css';
</style>