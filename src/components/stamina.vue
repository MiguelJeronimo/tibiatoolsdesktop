<script>
export default {
    name: 'stamina_tibia',
    data(){
        return {
            tiempo:'',
            rango:':'
        }
    }, methods:{
        OnClick() {
            let tiempo = this.$refs.tiempo.value
            console.log(tiempo)
            /**separaremos las horas de los minutos */
            let separador = tiempo.split(":");
            let hora = separador[0];
            let minuto = separador[1];
            let patron = RegExp(/^([01]?[0-9]|2[0-9]|3[0-9]|4[0-2]):[0-5][0-9]$/);
            if (patron.test(tiempo)) {
                let minutos_stamina = convertirHoraMinutosStamina(hora, minuto);
                let calculo_stamina_minutos = minutosDeStamina(minutos_stamina);
                this.rango = convertirMinutosReales(calculo_stamina_minutos);
            } else {
                this.rango = ':'
            }

        }
    }
}


    /**
 * hagamos los calculos correspondientes.
 * primero vamos a calcular cuantas horas tardara en recargar stamina
 * Tomando en cuenta que por cada 1 minuto de stamina es igual a 6 minutos reales
 * Recuerda que la stamina tardara 10 minutos para empezar a recargar despues de loguear
 */

 /**
  * Esta funcion retornara el total de horas que hemos gastado de stamina
  * convertida en minutos
  */
  function convertirHoraMinutosStamina(Hora, Minutos) {
    /**42:00 es cuando la estamina se encuentra llena */
    let hora_minutos,topStaminaMinutos,minutosTotales,totalMinutosStamina;    
    hora_minutos = Hora*60;
    topStaminaMinutos = 42*60;
    minutosTotales = parseInt(hora_minutos) + parseInt(Minutos);
    totalMinutosStamina = topStaminaMinutos - minutosTotales;
    return totalMinutosStamina;
}

/**
 * Calculando la stamina
 */
function minutosDeStamina(minutosStamina) {
    let tiempoReal;
    tiempoReal = (minutosStamina*6)+10;
    return tiempoReal;
}

/**
 * convertiremos los minutos reales a formato de hora
 */

 function horasReales(minutos) {
     let minutosHora;
     minutosHora = minutos*60;
     return minutosHora;
 }

function convertirMinutosReales(minutos) {
    let hora_minutos, minutosRestantes;
    hora_minutos = parseInt(minutos/60);
    minutosRestantes = minutos - horasReales(hora_minutos);
    return hora_minutos+':'+minutosRestantes;
}
</script>
<template>
    <main class="mdl-layout__content barraOpciones">

      <div class="page-content"><!-- Your content goes here -->
        <div class="titulo"><h4>Stamina</h4></div>
        <div class="titulo">
            <!-- Textfield with Floating Label -->
        <form >
        <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label" id="formulario">
            <input class="mdl-textfield__input" type="text" ref="tiempo" id="tiempo" pattern="^([01]?[0-9]|2[0-9]|3[0-9]|4[0-2]):[0-5][0-9]$">
            <label class="mdl-textfield__label blanco" for="sample3">Ingresar la hora de stamina</label>
            <span class="mdl-textfield__error" id="leyenda-error">El dato ingresado no es valido</span>
        </div>
         <!-- Accent-colored raised button with ripple -->
         <button v-on:click.prevent="OnClick()" id="calcular" class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--accent--purple separador-boton">
          Calcular
         </button>
        </form>
        </div>
      </div>

      <div class="mdl-grid contenedorTarjetas">
      <div class="mdl-cell mdl-cell--4-col">
        <div class="demo-card-wide mdl-card mdl-shadow--2dp">
        <div class="mdl-card__title">
          <h2 id="rangoMenor" ref="rango">{{rango}}</h2>
        </div>
        <div class="mdl-card__supporting-text">
         Tiempo en que tardará la stamina e recargar
        </div>
      </div>
      </div>
    </div>
    </main>
</template>
<style scoped>
    @import '../assets/css/stamina.css';
</style>