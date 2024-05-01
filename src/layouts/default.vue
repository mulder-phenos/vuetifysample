<template>
  <v-app>
    <v-navigation-drawer v-model="drawer">
      <div>
        <h1>Hi</h1>
      </div>
      <MainLeftMenu />
    </v-navigation-drawer>
    <v-app-bar>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-app-bar-title>Application</v-app-bar-title>
    </v-app-bar>
    <v-main>
      <router-view />
    </v-main>

    <AppFooter />
  </v-app>
</template>

<script setup>
import MainLeftMenu from '@/components/MainLeftMenu.vue';

//
const drawer = ref(null)
</script>

<script>
export default {
  data: () => ({ drawer: null }),
}
</script>
