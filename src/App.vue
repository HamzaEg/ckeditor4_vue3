<template>
  <v-layout>
    <v-app-bar color="grey-lighten-2">
      <v-app-bar-nav-icon
          variant="text"
          @click.stop="drawer = !drawer"
        ></v-app-bar-nav-icon>
    </v-app-bar>
    <v-navigation-drawer color="grey-darken-2" permanent v-model="drawer">

      <v-card class="mx-auto" max-width="300">
        <v-list density="compact">

          <v-list-item v-for="(item, i) in navItems" :key="i" :value="item" active-color="primary" @click="selectNav(i)">
            <template v-slot:prepend>
              <v-icon :icon="item.icon"></v-icon>
            </template>

            <v-list-item-title v-text="item.text"></v-list-item-title>
          </v-list-item>
        </v-list>

      </v-card>

    </v-navigation-drawer>
    <v-main>
      <v-card height="200px"></v-card>
      <ck-editor4 v-show="isCkEditor" />
    </v-main>
  </v-layout>
</template>

<script>
import CkEditor4 from './components/CkEditor4.vue';
import { ref } from "vue";


export default {
  name: 'App',

  components: {
    CkEditor4
  },

  setup() {
    const isCkEditor = ref(false);
    function selectNav(i) {
      i === 1 ? isCkEditor.value = true : isCkEditor.value = false;
      console.log(i);

    }



    return { isCkEditor, selectNav }

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
