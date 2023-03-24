
<script>
export default {
    name: 'experiencia_compartida',
    data(){
        return{
            rangoMayor: '-',
            rangoMenor: '-',
            level: '',
            txtNivel:'',
            formulario:''
        }
    },
    methods:{
        KeyDown(){
            validacionCampoTexto(this.level)
        },
        OnClick(){
            let formulario = this.$refs.formulario
            let leyendaError = this.$refs.leyenda_error
            let patron = RegExp(/^\d+$/);
            let nivel_menor, nivel_mayor;
            //console.log(this.level)
            //let nivel = this.$refs.txtNivel
            if (this.level !== "") {
                if (patron.test(this.level)) {
                    nivel_menor = rangoMenor(this.level);
                    nivel_mayor = rangoMayor(this.level);
                    this.rangoMenor = nivel_menor;
                    this.rangoMayor = nivel_mayor;
                } else {
                    formulario.classList.add('is-focused', 'is-invalid', 'is-dirty');
                    leyendaError.classList.add('mdl-textfield__error-validacion');
                    //leyendaError.innerHTML = 'Ingrese el nivel';
                }
            } else{
                    formulario.classList.add('is-focused','is-invalid', 'is-dirty');
                    leyendaError.classList.add('mdl-textfield__error-validacion');
                    //leyendaError.innerHTML = 'Ingrese el nivel'; 
                    this.rangoMenor = '-';
                    this.rangoMayor = '-'; 
            }
        }
    }
}

/**
 * funciones para el calculo de los rangos de la experiencia compartida
 */
 function rangoMenor(nivel) {
     let rango_menor, funcionTecho;
     rango_menor = ((nivel*2)/3);
     funcionTecho = Math.ceil(rango_menor);
     return funcionTecho;
 }

 function rangoMayor(nivel) {
     let rango_mayor, funcionPiso;
     rango_mayor = ((nivel*3)/2);
     funcionPiso = Math.floor(rango_mayor);
     return funcionPiso;
 }

function validacionCampoTexto(nivel) {
    let formulario = document.getElementById("formulario");
    let leyendaError = document.getElementById("leyenda-error");
    let patron = RegExp(/^\d+$/);
    if (nivel === '') {
        formulario.classList.add('is-focused','is-invalid','is-dirty');
        leyendaError.classList.add('mdl-textfield__error-validacion');
        leyendaError.innerHTML = 'Ingrese el nivel';
    } else if (patron.test(nivel)) {
        leyendaError.innerHTML = 'Debe ingresar un numero entero';
    }
     else{
        formulario.classList.remove('is-focused','is-invalid');
        leyendaError.classList.remove('mdl-textfield__error-validacion');
        leyendaError.classList.add('mdl-textfield__error')
        leyendaError.innerHTML = 'Debe ingresar un numero entero';
    }    
}
</script>


<template>
    <main class="mdl-layout__content barraOpciones">

<div class="page-content"><!-- Your content goes here -->
  <div class="titulo"><h4>Experiencia compartida</h4></div>
  <div class="titulo">
      <!-- Textfield with Floating Label -->
  <form >
  <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label" ref="formulario" id="formulario">
      <input class="mdl-textfield__input" type="text" ref="txtNivel" id="nivel" pattern="[0-9]*" @keydown="event => KeyDown(level = event.target.value)">
      <label class="mdl-textfield__label blanco" for="sample3">Nivel</label>
      <span class="mdl-textfield__error" ref="leyenda_error" id="leyenda-error">Debe ingresar un numero entero</span>
  </div>
   <!-- Accent-colored raised button with ripple -->
   <button 
   v-on:click.prevent = "OnClick()"
   id="calcular" class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--accent--purple separador-boton">
    Calcular
   </button >
  </form>
  </div>
</div>

<div class="mdl-grid">
<div class="mdl-cell mdl-cell--4-col contenedorTarjetas">
  <div class="demo-card-wide mdl-card mdl-shadow--2dp">
  <div class="mdl-card__title">
    <h2 id="rangoMenor">{{rangoMenor}}</h2>
  </div>
  <div class="mdl-card__supporting-text">
   Nivel minimo
  </div>
</div>
</div>
<div class="mdl-cell mdl-cell--4-col contenedorTarjetas">
  <div class="demo-card-wide mdl-card mdl-shadow--2dp">
      <div class="mdl-card__title">
        <h2 id="rangoMayor">{{rangoMayor}}</h2>
      </div>
      <div class="mdl-card__supporting-text">
       Nivel máximo
      </div>
    </div>
</div>
</div>
</main>  
</template>
<style scoped>
   @import '../assets/css/experienciacompartida.css';
</style>