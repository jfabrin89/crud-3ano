<template>
  <div id="cabecalho">
    <Menu :gaveta="inGaveta" />
    <v-toolbar color="black darken-3" dark app :clipped-left="$vuetify.breakpoint.mdAndUp" fixed>
      <v-toolbar-title style="width: 300px" class="ml-0 pl-3">
        <v-toolbar-side-icon @click.stop="inGaveta = !inGaveta"></v-toolbar-side-icon>
        <span class="hidden-sm-and-down">Crud 3º Ano - ADS UNIPAR</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <div class="usuario hidden-sm-and-down">
        <v-avatar size="32px" tile class="mr-3">
          <img src="../assets/logo.png" alt="Vuetify">
        </v-avatar>
        <span>NOME USUÁRIO</span>
      </div>
    </v-toolbar>
  </div>
</template>

<script>
import Menu from '@/components/Menu'
export default {
  components: {
    Menu
  },
  data () {
    return {
      inGaveta: true
    }
  },
  name: 'cabecalho'
}
</script>

<style>

</style>
