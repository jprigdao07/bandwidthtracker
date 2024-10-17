<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar class="toolbar-bg">
        <q-toolbar-title>
          Remote Bandwidth Tracker
        </q-toolbar-title>

        <q-btn class="menu" flat label="Home" color="white" @click="navigateTo('home')" />
        <q-btn  class="menu" flat label="Features" color="white" @click="navigateTo('features')" />
        <q-btn  class="menu" flat label="Services" color="white" @click="navigateTo('services')" />

      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

    
  <q-footer>
    <div class="text-center q-pa-sm">
        All Rights Reserved © 2024
      </div>
      </q-footer>

  </q-layout>

</template>

<script setup>
import { ref } from 'vue'

const leftDrawerOpen = ref(false)

function toggleLeftDrawer () {
  leftDrawerOpen.value = !leftDrawerOpen.value
}
</script>

<style scoped>

</style>
