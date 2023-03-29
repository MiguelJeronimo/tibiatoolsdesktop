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
            spinner_loadin: false,
            tablas: false,
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
            houses: '',
            muertes: '',
            information: '',
            otros_personajes:'',
            status_personaje:''
        }
    },
    methods:{
        BuscarPersonaje(){
            let nombre = this.$refs.ref_nombre
            if(nombre.value !== ''){
              this.spinner_loadin= true
              this.mensaje_error = false
              this.tabla_personajes = false
              this.mensaje_error=false
              this.tabla_casas = false
              this.tabla_muertes = false
              this.tabla_informacion = false
              this.tabla_otros_personajes = false
              let BuscarPersonaje = buscarPersonaje(nombre.value).then((data)=>{
                let name = data.characters.character.name;
                    if(name ==''){
                        this.tabla_personajes = false
                        this.mensaje_error=true
                        this.tabla_casas = false
                        this.tabla_muertes = false
                        this.tabla_informacion = false
                        this.tabla_otros_personajes = false
                        this.tablas = true
                        //ocultar el spinner
                        this.spinner_loadin = false
                    } else{
                        //ponemos en true la tabla de personajes para que se pinte
                        this.tabla_personajes = true
                        this.mensaje_error = false
                        this.tabla_casas = false
                        this.tabla_muertes = false
                        this.tabla_informacion = false
                        this.tabla_otros_personajes = false
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
                        // Validar si el personaje tiene casa
                        let casa = data.characters.character.houses
                        this.houses =  Houses(data.characters.character.houses)
                        if (casa == undefined) {
                          this.tabla_casas = false
                        } else{
                          this.tabla_casas = true
                        }
                        //validando si el personaje tiene muertes
                        let muertes = data.characters.deaths
                        this.muertes = Muertes(muertes)
                        if (muertes == undefined) {
                          this.tabla_muertes = false
                        } else{
                          this.tabla_muertes = true
                        }
                        //validando la información de la cuenta del personaje
                        let account_information = data.characters.account_information
                        this.information = accountInformation(account_information)
                        if (account_information === '') {
                          this.tabla_informacion = false
                        } else{
                          this.tabla_informacion = true
                        }
                        //validando si tiene mas peronsajes en la cuenta
                        let otros_personajes = data.characters.other_characters
                        this.otros_personajes = otrosPersonajes(otros_personajes)
                        if (otros_personajes == undefined) {
                          this.tabla_otros_personajes = false
                        } else{
                          this.tabla_otros_personajes = true
                        }
                        this.tablas = true
                        //ocultar el spinner
                        this.spinner_loadin = false
                    }   
                })
                setTimeout(() => {
                  BuscarPersonaje
                }, 3000);
            } else{
                let formulario = this.$refs.formulario
                let leyendaError = this.$refs.leyenda_error
                formulario.classList.add('is-focused','is-invalid','is-dirty')
                leyendaError.classList.add('mdl-textfield__error-validacion')
                leyendaError.innerHTML = 'Ingrese el nombre del personaje'
                this.tabla_personajes= false
                this.tabla_casas = false
                this.tabla_muertes = false
                this.tabla_informacion = false
                this.tabla_otros_personajes = false
                this.table_mundos = true
                //ocultar el spinner
                this.spinner_loadin = false
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

function Houses(houses) {
  let plantilla = new Array()
  if(houses !== undefined){
    houses.forEach(casas => {
      plantilla.push(`${casas.name}, ${casas.town}, ${casas.paid}`)
    })
    return plantilla
  } else{
      return plantilla = ""
  }
}

function Muertes(muertes) {
  if(muertes !== undefined){
      return muertes
  } else{
      return muertes = ""
  }
}

function accountInformation(informacion) {
  if(informacion.created !== undefined){
    console.log(informacion)  
    return informacion
  } else{
    return informacion = ''
  }
}
function otrosPersonajes(personajes) {
  if(personajes !== undefined){
    return personajes
  } else{
    return personajes = ''
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
   </button>
  </form>
  </div>
</div>

  <div class="">
    <h5 class="titulo-tabla">Información del personaje</h5>
  </div >
  
 <div class="tabla" v-if="spinner_loadin == true">
  <div class="spinner" style="text-align: center;"></div>
 </div>
<div class="tabla" id="tabla" v-if="tablas == true">
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

<div class="tabla" id="tabla-houses" v-if="tablas == true">
  <table class="mdl-data-table mdl-js-data-table with=1000px" v-if="tabla_casas===true">
          <caption>Casas</caption>
          <tbody id="tabla">
            <tr v-for="house in houses" :key="house">
              <td class="mdl-data-table__cell--non-numeric">House</td>
              <td class="mdl-data-table__cell--non-numeric">{{house}}</td>
            </tr>
          </tbody>
    </table>
</div>
<div class="tabla" id="tabla-muertes" v-if="tablas == true"> 
  <table class="mdl-data-table mdl-js-data-table with=1000px" v-if="tabla_muertes===true">
          <caption>Muertes</caption>
          <tbody id="tabla">
            <tr v-for="muerte in muertes" :key="muerte">
              <td class="mdl-data-table__cell--non-numeric">level: {{muerte.level}}</td>
              <td class="mdl-data-table__cell--non-numeric">{{muerte.time}}</td>
              <td class="mdl-data-table__cell--non-numeric">{{muerte.reason}}</td>
            </tr>
          </tbody>
    </table>
</div>
<div class="tabla" id="tabla-informacion" v-if="tablas == true"> 
  <table class="mdl-data-table mdl-js-data-table with=1000px" v-if="tabla_informacion===true">
          <caption>Información de la cuenta</caption>
          <tbody id="tabla">
            <tr>
              <td class="mdl-data-table__cell--non-numeric">Created:</td>
              <td class="mdl-data-table__cell--non-numeric">{{information.created}}</td>
            </tr>
            <tr>
              <td class="mdl-data-table__cell--non-numeric">Loyalty Title:</td>
              <td class="mdl-data-table__cell--non-numeric">{{information.loyalty_title}}</td>
            </tr>
          </tbody>
    </table>
</div>
<div class="tabla" id="tabla-otros-personajes" v-if="tablas == true">
  <table class="mdl-data-table mdl-js-data-table with=1000px" v-if="tabla_otros_personajes===true">
          <caption>Otros personajes</caption>
          <thead>
          <tr>
            <th class="mdl-data-table__cell--non-numeric">Nombre</th>
            <th class="mdl-data-table__cell--non-numeric">Mundo</th>
            <th class="mdl-data-table__cell--non-numeric">Status</th>
            <th class="mdl-data-table__cell--non-numeric">Deleted</th>
            <th class="mdl-data-table__cell--non-numeric">Main</th>
            <th class="mdl-data-table__cell--non-numeric">Traded</th>
          
          </tr>
        </thead>
          <tbody id="tabla">
          <tr v-for="info_personaje in otros_personajes" :key="info_personaje">
            <td class="mdl-data-table__cell--non-numeric">{{info_personaje.name}}</td>
            <td class="mdl-data-table__cell--non-numeric">{{info_personaje.world}}</td>
            <td class="mdl-data-table__cell--non-numeric">{{info_personaje.status}}</td>
            <td class="mdl-data-table__cell--non-numeric">{{info_personaje.deleted}}</td>
            <td class="mdl-data-table__cell--non-numeric">{{info_personaje.main}}</td>
            <td class="mdl-data-table__cell--non-numeric">{{info_personaje.traded}}</td>
          </tr> 
        </tbody>
    </table> 
</div>

</main>
</template>
<style scoped>
 @import '../assets/css/spinner.css';
 @import '../assets/css/character.css';

</style>