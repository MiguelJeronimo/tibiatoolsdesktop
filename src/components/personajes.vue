<script>
export default {
    name: 'personajes_tibia',
}
</script>

<template>
<main class="mdl-layout__content barraOpciones  tablas contenedor">

<div class="page-content"><!-- Your content goes here -->
  <div class="titulo"><h4>Personaje</h4></div>
  <div class="titulo">
      <!-- Textfield with Floating Label -->
  <form >
  <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label" id="formulario">
      <input class="mdl-textfield__input" type="text" id="nombre">
      <label class="mdl-textfield__label blanco" for="sample3">Character</label>
      <span class="mdl-textfield__error" id="leyenda-error"></span>
  </div>
   <!-- Accent-colored raised button with ripple -->
   <button id="calcular" class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--accent--purple separador-boton">
    Buscar
   </button >
  </form>
  </div>
</div>

  <div class="">
    <h5 class="titulo-tabla">Información del personaje</h5>
  </div >
  

<div class="tabla" id="tabla"> 
</div>

<div class="tabla" id="tabla-houses"> 
</div>
<div class="tabla" id="tabla-muertes"> 
</div>
<div class="tabla" id="tabla-informacion"> 
</div>
<div class="tabla" id="tabla-otros-personajes"> 
</div>

</main>
</template>
<style scoped>
 @import '../assets/css/character.css';

</style>