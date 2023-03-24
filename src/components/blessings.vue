<script>
export default {
    name: 'blessings_vista',
    data(){
        return {
            level: '',
            spiritual: '--',
            embrace: '--',
            suns: '--',
            solitude: '--',
            phoenix: '--',
            twits: '--',
            heart: '--',
            blood: '--',
            total: '--',
            totalbless: '',
            totalHeart: '',
            totalBlood: ''
        }
    },
    methods:{
        eventoTeclado(){
            //validaremos por evento de teclado si el formato ingresado es correcto
            validacionCampoTexto(this.level)
        },
        OnClick(evento){
            let bless;
            let totalblessings
            let blessEspecial
            let blessEspecial2
            let nivel = this.level
            let checkedHeart = this.$refs.switch_1
            let checkedBlood = this.$refs.switch_2
            totalblessings=0;
            if (evento == 'click') {
                let patron = RegExp(/^\d+$/);
                validacion(nivel);
                if (patron.test(nivel)) {
                    bless = blessIndividual(nivel);
                    this.spiritual = new Intl.NumberFormat("en-IN").format(bless) + ' gold(s)';
                    this.embrace = new Intl.NumberFormat("en-IN").format(bless) + ' gold(s)';
                    this.suns = new Intl.NumberFormat("en-IN").format(bless) + ' gold(s)';
                    this.solitude = new Intl.NumberFormat("en-IN").format(bless) + ' gold(s)';
                    this.phoenix = new Intl.NumberFormat("en-IN").format(bless) + ' gold(s)';
                    this.twits = new Intl.NumberFormat("en-IN").format(bless) + ' gold(s)';
                    totalblessings = bless * 6;
                    this.totalbless = totalblessings
                    this.total = new Intl.NumberFormat("en-IN").format(totalblessings) + ' gold(s)';
                } else {
                    this.spiritual = '--';
                    this.embrace = '--';
                    this.suns = '--';
                    this.solitude = '--';
                    this.phoenix = '--';
                    this.twits = '--';
                    this.total = '--';
                    totalblessings = 0;
                    this.totalbless = totalblessings
                }
            }
            else if(evento == 'change'){  
                //validamos si esta checkeado
                if (checkedHeart.checked) {

                    if (nivel != '') {
                        blessEspecial = blessingEspecial(nivel);
                        this.totalHeart = blessEspecial
                        this.heart = new Intl.NumberFormat("en-IN").format(blessEspecial) + ' gold(s)';
                    } else {
                        Errores();
                        this.heart = '--';
                        totalblessings = 0;
                        this.totalbless = totalblessings
                    }
                } else {
                    this.heart = "--";
                    blessEspecial = 0;
                    this.totalHeart = blessEspecial
                }
                //validamos si esta checkeado
                if (checkedBlood.checked) {
                    if (nivel != '') {
                        blessEspecial2 = blessingEspecial(nivel);
                        this.totalBlood = blessEspecial2
                        this.blood = new Intl.NumberFormat("en-IN").format(blessEspecial2) + ' gold(s)'
                    } else {
                        Errores();
                        this.blood = '--'
                        totalblessings = 0
                        this.totalbless = totalblessings
                    }
                } else {
                    this.blood = "--";
                    blessEspecial2 = 0;
                    this.totalBlood = blessEspecial2
                }

                if (isNaN(totalblessings)) {
                    totalblessings = 0
                    this.totalbless = totalblessings
                }

           }
           this.total = new Intl.NumberFormat("en-IN")
                .format(this.totalbless + this.totalHeart + this.totalBlood) + ' gold(s)';
        }
    }
}

function validacion(nivel) {
    let formulario = document.getElementById("formulario");
    let leyendaError = document.getElementById("leyenda-error");
    let spiritual = document.getElementById("spiritual");
    let embrace = document.getElementById("embrace");
    let suns = document.getElementById("suns");
    let solitude = document.getElementById("solitude");
    let phoenix = document.getElementById("phoenix");
    let twits = document.getElementById("twits");
    if (nivel == '') {
        formulario.classList.add('is-focused','is-invalid', 'is-dirty');
        leyendaError.classList.add('mdl-textfield__error-validacion');
        leyendaError.innerHTML = 'El campo esta vacio';
        spiritual.innerHTML = '--'; 
        embrace.innerHTML = '--';
        suns.innerHTML = '--';
        solitude.innerHTML = '--';
        phoenix.innerHTML = '--';
        twits.innerHTML = '--';
    }
}

function Errores() {
    let formulario = document.getElementById("formulario");
    let leyendaError = document.getElementById("leyenda-error");
    formulario.classList.add('is-focused','is-invalid', 'is-dirty');
    leyendaError.classList.add('mdl-textfield__error-validacion');
    leyendaError.innerHTML = 'El campo esta vacio'; 
}

function validacionCampoTexto(nivel) {
    let formulario = document.getElementById("formulario");
    let leyendaError = document.getElementById("leyenda-error");
    let patron = RegExp(/^\d+$/);
    if (nivel === '') {
        formulario.classList.add('is-focused','is-invalid','is-dirty');
        leyendaError.classList.add('mdl-textfield__error-validacion');
        leyendaError.innerHTML = 'El campo esta vacio';
    } else if (patron) {
        leyendaError.innerHTML = 'Debe ingresar un numero entero';
    }
     else{
        formulario.classList.remove('is-focused','is-invalid');
        leyendaError.classList.remove('mdl-textfield__error-validacion');
        leyendaError.classList.add('mdl-textfield__error')
        leyendaError.innerHTML = 'Debe ingresar un numero entero';
    }    
}

/**
 * Esta funcion calcula el costo unitario de las blessings principales.
 * Incluyendo el costo de la twist of fate
 */
 function blessIndividual(nivel) {
    const constante = 200;
    let blessing;
    if (nivel <= 30) {
         blessing = 2000;
    } else if (nivel >= 31 && nivel < 120) {
        blessing = constante * (nivel - 20);
    } else{
        blessing = 20000;
    }
    return blessing;
}

/**
 * Esta funcion calcula el costo unitario de los bless Heart of mountain
 * y Blood of mountain
 */
 function blessingEspecial(nivel) {
    const constante = 260;
    let blessing_especial;
    if (nivel <=30) {
        blessing_especial = 2600;
    } else if (nivel >=30 && nivel < 120) {
        blessing_especial = constante * (nivel - 20);
    } else{
        blessing_especial = 26000;
    }
    return blessing_especial;   
}


</script>

<template>
    <main class="mdl-layout__content barraOpciones tablas contenedor">

<div class="page-content"><!-- Your content goes here -->
  <div class="titulo"><h4>Blessings</h4></div>
  <div class="titulo">
      <!-- Textfield with Floating Label -->
  <form >
  <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label" id="formulario">
      <input class="mdl-textfield__input" type="text" id="nivel" pattern="[0-9]*" @keydown="event => eventoTeclado(level = event.target.value)">
      <label class="mdl-textfield__label blanco" for="sample3">Nivel</label>
      <span class="mdl-textfield__error" id="leyenda-error">Debe ingresar un numero entero</span>
  </div>
   <!-- Accent-colored raised button with ripple -->
   <button 
   v-on:click.prevent = "$event =>OnClick($event.type)"
   id="calcular" class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--accent--purple separador-boton">
    Calcular
   </button>
  </form>
  </div>
</div>

<div  class="mdl-grid titulo">
  <div class="mdl-cell mdl-cell--4-col">Spiritual:</div>
  <div class="mdl-cell mdl-cell--4-col" id="spiritual">{{spiritual}}</div>

  <div class="mdl-cell mdl-cell--4-col">Embrace:</div>
  <div class="mdl-cell mdl-cell--4-col" id="embrace">{{embrace}}</div>
  
  <div class="mdl-cell mdl-cell--4-col">Suns:</div>
  <div class="mdl-cell mdl-cell--4-col" id = "suns">{{suns}}</div>

  <div class="mdl-cell mdl-cell--4-col">Solictude:</div>
  <div class="mdl-cell mdl-cell--4-col" id="solitude">{{solitude}}</div>

  <div class="mdl-cell mdl-cell--4-col">Phoenix:</div>
  <div class="mdl-cell mdl-cell--4-col" id="phoenix">{{phoenix}}</div>
</div>
 
<div class="mdl-grid ">
  <div class="mdl-cell mdl-cell--5-col titulo">Twits of fate:</div>
  <div class="mdl-cell mdl-cell--2-col etiqueta" id="twits">{{twits}}</div>
</div>

<div class="mdl-grid caja">
  <div class="mdl-cell mdl-cell--3-col-phone">
    <label class="mdl-switch mdl-js-switch mdl-js-ripple-effect" for="switch-1">
      <input type="checkbox" id="switch-1" ref="switch_1" class="mdl-switch__input"  @change="$event => OnClick($event.type)">
      <span class="mdl-switch__label blanco">Heart of Mountain:</span>
    </label>
  </div>
  
  <div class="mdl-cell mdl-cell--2-col-phone blanco" id="heart">{{heart}}</div>
  
  <div class="mdl-cell mdl-cell--3-col-phone">
    <label class="mdl-switch mdl-js-switch mdl-js-ripple-effect" for="switch-2">
      <input type="checkbox" id="switch-2" ref="switch_2" class="mdl-switch__input" @change="$event => OnClick($event.type)">
      <span class="mdl-switch__label blanco">Blood of Mountain:</span>
    </label>
  </div>

  <div class="mdl-cell mdl-cell--2-col-phone blanco" id="blood">{{blood}}</div>
</div>

<div class="mdl-grid caja">
  <div class="mdl-cell mdl-cell--3-col titulo">Total:</div>
  <div class="mdl-cell mdl-cell--3-col etiqueta" id="total">{{total}}</div>
</div>
</main>
</template>
