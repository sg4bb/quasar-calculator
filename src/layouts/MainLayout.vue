<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title class="text-weight-bold">
          <q-icon name="calculate" style="padding-bottom: 4px;"></q-icon> Calculadora
        </q-toolbar-title>

        <div style="font-size: 10px; padding-top: 11px;">Desarrollada por @sg4bb</div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
</script>
