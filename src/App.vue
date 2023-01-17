<template>
  <v-layout>
    <v-app-bar color="grey-lighten-2">
      <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
    </v-app-bar>
    <v-navigation-drawer color="grey-darken-2" permanent v-model="drawer">

      <v-card class="mx-auto" max-width="300">
        <v-list density="compact">

          <v-list-item v-for="(item, i) in navItems" :key="i" :value="item" active-color="primary"
            @click="selectNav(i, item)">
            <template v-slot:prepend>
              <v-icon :icon="item.icon"></v-icon>
            </template>

            <v-list-item-title v-text="item.text"></v-list-item-title>
          </v-list-item>
        </v-list>

      </v-card>

    </v-navigation-drawer>
    <v-main>
      <v-card height="3em" class="d-flex justify-center bg-purple-lighten-3">
        <template v-slot:title>
       {{ navTitle }}
    </template>
      </v-card>
      <DokumentenManagement v-show="isDocManag"/>
      
    </v-main>
  </v-layout>
</template>

<script>
import DokumentenManagement from './components/DokumentenManagement.vue';
import { ref } from "vue";


export default {
  name: 'App',

  components: {
    DokumentenManagement,
  },

  setup() {
    const isDocManag = ref(false);
    const navTitle = ref("");
    function selectNav(i, navItem) {
      i === 1 ? isDocManag.value = true : isDocManag.value = false;
      navTitle.value = navItem.text
      console.log(i);

    }



    return { isDocManag, selectNav, navTitle }

  },

  data: () => ({
    drawer: false,
    navItems: [
      { text: 'Nursit Institute', icon: 'mdi-home' },
      { text: 'Dokumentverwaltung', icon: 'mdi-file-document-edit-outline' }
    ],
  }),
}
</script>
