<template>
    <v-app>
<v-container>


  <v-card 
    class="CoverBook"
    color="surface-variant"
    image="https://cdn.vuetifyjs.com/docs/images/cards/dark-beach.jpg"
    max-width="500"
    subtitle="Take a walk down the beach"
    title="Evening sunset"
  >
    <template v-slot:actions>
      <v-btn
      class="Btn1"
        append-icon="mdi-chevron-right"
        color="red-lighten-2"
        text="Book Activity"
        variant="outlined"
        block
      ></v-btn>
    </template>
  </v-card>







</v-container>

</v-app class="app">
</template>

<style scoped>
.app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* Höhe des Viewports */
  margin: 0;
}

.CoverBook {
  display: flex;
  justify-content: center;
  align-items: center;
  
  width: 800px;   /* neue Breite */
  height: 600px;  /* neue Höhe */
  
  background-color: #f0f0f0; /* optional zur Sichtbarkeit */
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2); /* optionaler Stil */
}

</style>