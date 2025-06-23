<template>
    <v-container fluid class="pa-8 full-height-container">
      <v-card
        class="mx-auto book-card"
        max-width="1200"
        elevation="8"
        rounded="xl"
      >
        <v-card-text class="pa-12">
          <v-row no-gutters>
            <!-- Buchcover Sektion -->
            <v-col cols="12" md="4" class="d-flex justify-center align-center">
              <v-card
                class="book-cover"
                width="280"
                height="400"
                elevation="4"
                rounded="lg"
              >
                <v-img
                  :src="bookCover"
                  height="100%"
                  cover
                  class="book-image"
                >
                  <template v-slot:placeholder>
                    <div class="d-flex align-center justify-center fill-height">
                      <v-icon size="64" color="grey-lighten-2">
                        mdi-book-open-variant
                      </v-icon>
                    </div>
                  </template>
                </v-img>
              </v-card>
            </v-col>
          </v-row>
        </v-card-text>
      </v-card>
    </v-container>


    <v-col cols="12" md="8" class="d-flex flex-column align-center pl-6 pl-md-12">
            <!-- Audio Player Bereich -->
            <v-card
              class="audio-player-card fixed-width"
              elevation="2"
              rounded="xl"
              color="grey-lighten-3"
            >
              <v-card-text class="pa-6">
                <v-row align="center" no-gutters>
                  <v-col cols="auto" class="mr-3">
                    <v-btn
                      :icon="isPlaying ? 'mdi-pause' : 'mdi-play'"
                      size="large"
                      color="primary"
                      @click="togglePlay"
                    />
                  </v-col>
                  <v-col>
                    <v-slider
                      v-model="currentTime"
                      :max="duration"
                      hide-details
                      color="primary"
                      track-color="grey-lighten-2"
                      thumb-color="primary"
                      @input="seekAudio"
                    >
                      <template v-slot:prepend>
                        <span class="text-caption">{{ formatTime(currentTime) }}</span>
                      </template>
                      <template v-slot:append>
                        <span class="text-caption">{{ formatTime(duration) }}</span>
                      </template>
                    </v-slider>
                  </v-col>
                </v-row>
              </v-card-text>
            </v-card>

          
            <v-card
              class="placeholder-card-large fixed-width mt-4"
              height="200"
              elevation="1"
              rounded="xl"
              color="grey-lighten-4"
            >
              <v-card-text class="d-flex flex-column align-center justify-center h-100">
                <div class="text-h5 text-grey-darken-1 mb-2">PLACE HOLDER</div>
                <div class="text-body-1 text-grey-darken-2">PLACE HOLDER</div>
              </v-card-text>
            </v-card>
          </v-col>
  










  </template>
  
  <script setup>
    const bookCover = 'https://via.placeholder.com/280x400';
  </script>
