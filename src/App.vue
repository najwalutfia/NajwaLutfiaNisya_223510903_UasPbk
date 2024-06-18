<template>
  <q-layout view="hHh lpR fFf">
    <q-header class="header">
      <q-toolbar>
        <q-toolbar-title>
          UAS Najwa Lutfia Nisya
        </q-toolbar-title>
        <q-btn-group flat class="rout-nav">
          <router-link to="/todos">
            <q-btn flat :outline="!isActive('/todos')" label="Todos" />
          </router-link>
          <router-link to="/posts">
            <q-btn flat :outline="!isActive('/posts')" label="Posts" />
          </router-link>
          <router-link to="/albums">
            <q-btn flat :outline="!isActive('/albums')" label="Albums" />
          </router-link>
        </q-btn-group>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <q-page>
        <router-view />
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import { useRoute } from 'vue-router';

export default {
  setup() {
    const route = useRoute();
    const isActive = (path) => route.path === path;
    return {
      isActive
    };
  }
};
</script>

<style>
* {
  font-family: 'Gill Sans', 'Gill Sans MT','Trebuchet MS', sans-serif;
}

body {
  background-color: #e3fcff;
}

.header {
  background-color: #eefdff !important;
  color: #004d40 !important;
  padding: 10px;
  box-shadow: 5px 5px 5px #00332e4b;
}

.rout-nav button {
  color: #004d40;
  font-size: 16px;
}
</style>
