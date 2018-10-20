<template>
  <div id="menu">
    <v-navigation-drawer fixed :clipped="$vuetify.breakpoint.mdAndUp" app v-model="gaveta" class="mt-4">
      <v-list dense class="mt-5">
        <template v-for="item in lstMenu">
          <v-list-group v-if="item.filhos" v-model="item.aberto" :key="item.texto" :prepend-icon="item.aberto ? item.icone : item['icon-alt']" append-icon="">
            <v-list-tile slot="activator">
              <v-list-tile-content>
                <v-list-tile-title>
                  {{ item.texto }}
                </v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
            <v-list-tile v-for="(filho, i) in item.filhos" :key="i">
              <div @click="$router.push(item.link)">
                <v-list-tile-action v-if="filho.icone">
                  <v-icon>{{ filho.icone }}</v-icon>
                </v-list-tile-action>
                <v-list-tile-content>
                  <v-list-tile-title>
                    {{ filho.texto }}
                  </v-list-tile-title>
                </v-list-tile-content>
              </div>
            </v-list-tile>
          </v-list-group>
          <v-list-tile v-else @click="$router.push(item.link)" :key="item.texto">
            <v-list-tile-action>
              <v-icon>{{ item.icone }}</v-icon>
            </v-list-tile-action>
            <v-list-tile-content>
              <v-list-tile-title>
                {{ item.texto }}
              </v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
        </template>
      </v-list>
    </v-navigation-drawer>
  </div>
</template>

<script>
export default {
  data () {
    return {
      dialog: false,
      lstMenu: [
        { icone: 'home', texto: 'Pagina Inicial', link: 'home' },
        { icone: 'person', texto: 'Cliente', link: 'cliente' },
        { icone: 'local_offer', texto: 'Produto', link: 'produto' },
        { icone: 'local_grocery_store', texto: 'Pedidos', link: 'pedidos' }
      ]
    }
  },
  props: [
    'gaveta'
  ]
}
</script>

<style>

</style>
