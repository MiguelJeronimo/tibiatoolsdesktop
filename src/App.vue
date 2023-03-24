<template>
<div class="mdl-layout mdl-js-layout mdl-layout--fixed-header">
<menu_inicio></menu_inicio>
</div>
{{material_script}}
</template>

<script>
import menu_inicio from "./components/menu.vue";
export default {
  name: 'App',
  components: {
    menu_inicio,
  }
}
</script>
