<template>
  <v-app-bar :elevation="2" color="indigo">
    <template v-slot:prepend>
      <v-app-bar-nav-icon></v-app-bar-nav-icon>
    </template>

    <v-app-bar-title>音ゲーマーレーティング</v-app-bar-title>

    <template v-slot:append>
      <v-btn icon="mdi-github" href="https://github.com/3kanAlpha/otogamer-rating" target="_blank"></v-btn>
    </template>
  </v-app-bar>
</template>