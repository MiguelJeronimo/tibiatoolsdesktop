<script>
export default {
    name: 'personajes_tibia',
    data(){
        return {
            ref_nombre:'',
            mensaje_error: false,
            tabla_personajes: false,
            tabla_casas: false,
            tabla_muertes: false,
            tabla_informacion:false,
            tabla_otros_personajes: false,
            name:'',
            title:'',
            sex:'',
            Achievement_Points:'',
            world:'',
            residence:'',
            casado:'',
            guilds:'',
            Last_Login:'',
            comment:'',
            Account_Status:'',
        }
    },
    methods:{
        BuscarPersonaje(){
            let nombre = this.$refs.ref_nombre
            if(nombre.value !== ''){
                buscarPersonaje(nombre.value).then((data)=>{
                    let name = data.characters.character.name;
                    if(name ==''){
                        this.tabla_personajes = false
                        this.mensaje_error=true
                    } else{
                        //ponemos en true la tabla de personajes para que se pinte
                        this.tabla_personajes = true
                        this.mensaje_error = false
                        this.name = name
                        this.title = data.characters.character.title;
                        this.sex = data.characters.character.sex;
                        this.vocation = data.characters.character.vocation;
                        this.level = data.characters.character.level;
                        this.Achievement_Points = data.characters.character.achievement_points;
                        this.world = data.characters.character.world;
                        this.residence = data.characters.character.residence;
                        let guild = data.characters.character.guild;
                        this.guilds = Guild(guild.rank,guild.name)
                        this.Last_Login = data.characters.character.last_login;
                        let comentario = data.characters.character.comment;
                        this.comment= Comentario(comentario)
                        this.Account_Status = data.characters.character.account_status;
                        this.house = data.characters.character.houses
                        this.muertes = data.characters.deaths
                        this.informacion = data.characters.account_information
                        this.otros_personajes = data.characters.other_characters
                        let casado = data.characters.character.married_to
                        this.casado = Casado(casado)           
                    }   
                })
            } else{
                let formulario = this.$refs.formulario
                let leyendaError = this.$refs.leyenda_error
                formulario.classList.add('is-focused','is-invalid','is-dirty')
                leyendaError.classList.add('mdl-textfield__error-validacion')
                leyendaError.innerHTML = 'Ingrese el nombre del personaje'
                this.tabla_personajes= false
            }
        }
    }
}
/**
 * 
 * @param {*} nombre -> nombre de personaje de tibia
 * @returns data del json 
 */
async function buscarPersonaje(nombre) {
    let url = `https://api.tibiadata.com/v3/character/${nombre}`;
    // Variables para extraer los datos de personaje
    let respuesta = fetch(url)
    let data = (await respuesta).json();
    return data
}

/**
 * Estas funciones validaran ciertos datos del personaje
 * que puede o no puede tener
 */

 function Comentario(comentario) {
    if (comentario !== undefined) {
      return comentario;      
    } else {
        return 'No tiene comentario';
    }
}
function Casado(comentario) {
  if (comentario !== undefined) {
    return "Casado con: "+comentario;      
  } else {
      return 'Soltero';
  }
}

function Guild(rango, nombreGuild) {
    if (rango !== undefined && nombreGuild !== undefined) {
        return rango +' of the '+nombreGuild;
    } else{
        return 'No tiene guild';
    }
}



</script>

<template>
<main class="mdl-layout__content barraOpciones  tablas contenedor">

<div class="page-content"><!-- Your content goes here -->
  <div class="titulo"><h4>Personaje</h4></div>
  <div class="titulo">
      <!-- Textfield with Floating Label -->
  <form >
  <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label" ref="formulario" id="formulario">
      <input class="mdl-textfield__input" type="text" id="nombre" ref="ref_nombre">
      <label class="mdl-textfield__label blanco" for="sample3">Character</label>
      <span class="mdl-textfield__error" ref="leyenda_error" id="leyenda-error"></span>
  </div>
   <!-- Accent-colored raised button with ripple -->
   <button 
   v-on:click.prevent="BuscarPersonaje()"
   id="calcular" class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--accent--purple separador-boton">
    Buscar
   </button >
  </form>
  </div>
</div>

  <div class="">
    <h5 class="titulo-tabla">Información del personaje</h5>
  </div >
  

<div class="tabla" id="tabla" >
    <!--Tabla de la data de los personajes-->
    <table class="mdl-data-table mdl-js-data-table with=1000px" v-if="tabla_personajes==true">
            <tbody id="tabla">
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Nombre</td>
                <td class="mdl-data-table__cell--non-numeric">{{name}}</td>
              </tr>
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Titulo</td>
                <td class="mdl-data-table__cell--non-numeric">{{title}}</td>
              </tr>
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Sexo:</td>
                <td class="mdl-data-table__cell--non-numeric">{{sex}}</td>
              </tr>
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Vocación</td>
                <td class="mdl-data-table__cell--non-numeric">{{vocation}}</td>
              </tr>
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Level</td>
                <td class="mdl-data-table__cell--non-numeric">{{level}}</td>
              </tr>
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Achievement Points:</td>
                <td class="mdl-data-table__cell--non-numeric">{{Achievement_Points}}</td>
              </tr>
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Mundo:</td>
                <td class="mdl-data-table__cell--non-numeric">{{world}}</td>
              </tr>
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Residente:</td>
                <td class="mdl-data-table__cell--non-numeric">{{residence}}</td>
              </tr>
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Situación sentimental:</td>
                <td class="mdl-data-table__cell--non-numeric">{{casado}}</td>
              </tr>
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Guild membership:</td>
                <td class="mdl-data-table__cell--non-numeric">{{guilds}}</td>
              </tr>
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Ultima conexión:</td>
                <td class="mdl-data-table__cell--non-numeric">{{Last_Login}}</td>
              </tr>
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Comentario:</td>
                <td class="mdl-data-table__cell--non-numeric texto" >{{comment}}</td>
              </tr>
              <tr>
                <td class="mdl-data-table__cell--non-numeric">Account Status:</td>
                <td class="mdl-data-table__cell--non-numeric">{{Account_Status}}</td>
              </tr>
            </tbody>
        </table>
        
        <spam v-if="mensaje_error==true">
            <strong class="error" id="error">Personaje no existe</strong>
        </spam>
</div>

<div class="tabla" id="tabla-houses" v-if="tabla_casas===true"> 
</div>
<div class="tabla" id="tabla-muertes" v-if="tabla_muertes===true"> 
</div>
<div class="tabla" id="tabla-informacion" v-if="tabla_informacion===true"> 
</div>
<div class="tabla" id="tabla-otros-personajes" v-if="tabla_otros_personajes===true"> 
</div>

</main>
</template>
<style scoped>

 @import '../assets/css/character.css';

</style>