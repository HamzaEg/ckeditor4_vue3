<template>
  <v-layout>
    <v-app-bar color="light-blue-darken-2">
      <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
    </v-app-bar>
    <v-navigation-drawer color="light-blue-lighten-5" permanent v-model="drawer">

      <v-card class="mx-auto" max-width="300">
        <v-list density="compact">

          <v-list-item v-for="(item, i) in navItems" :key="i" :value="item" active-color="blue-darken-3"
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
      <v-card height="3em" class="d-flex justify-center bg-light-blue-lighten-5">
        <template v-slot:title>
       {{ navTitle }}
    </template>
      </v-card>
      <DokumentenManagement v-if="isDocManag"/>
      
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
    const drawer = ref(false);
    const isDocManag = ref(false);
    const navTitle = ref("");
    function selectNav(i, navItem) {
      i === 1 ? isDocManag.value = true : isDocManag.value = false;
      navTitle.value = navItem.text;
      drawer.value = !drawer.value;
    }



    return { isDocManag, selectNav, navTitle, drawer }

  },

  data: () => ({
    
    navItems: [
      { text: 'Nursit Institute', icon: 'mdi-home' },
      { text: 'Dokumentverwaltung', icon: 'mdi-file-document-edit-outline' }
    ],
  }),
}
</script>
