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

          <!-- Rechte Seite mit Audio und Platzhaltern -->
          <v-col cols="12" md="8" class="pl-md-6">
            <v-row no-gutters class="fill-height">
              <!-- Audio Player Bereich -->
              <v-col cols="12" class="mb-4">
                <v-card
                  class="audio-player-card"
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
              </v-col>

              <!-- Platzhalter Bereiche -->
              <v-col cols="12">
                <v-row no-gutters>
                  <!-- Obere Platzhalter -->
                  <v-col cols="6" class="pr-2">
                    <v-card
                      class="placeholder-card"
                      height="80"
                      elevation="1"
                      rounded="lg"
                      color="grey-lighten-4"
                    >
                      <v-card-text class="d-flex align-center justify-center h-100">
                        <span class="text-grey-darken-2">Platzhalter</span>
                      </v-card-text>
                    </v-card>
                  </v-col>
                  <v-col cols="6" class="pl-2">
                    <v-card
                      class="placeholder-card"
                      height="80"
                      elevation="1"
                      rounded="lg"
                      color="grey-lighten-4"
                    >
                      <v-card-text class="d-flex align-center justify-center h-100">
                        <span class="text-grey-darken-2">Platzhalter</span>
                      </v-card-text>
                    </v-card>
                  </v-col>
                </v-row>
              </v-col>

              <!-- Großer Platzhalter unten -->
              <v-col cols="12" class="mt-4">
                <v-card
                  class="placeholder-card-large"
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
            </v-row>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: 'BookAudioCard',
  data() {
    return {
      isPlaying: false,
      currentTime: 0,
      duration: 300, // 5 Minuten als Beispiel
      bookCover: 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQwIiBoZWlnaHQ9IjM1MCIgdmlld0JveD0iMCAwIDI0MCAzNTAiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+CjxyZWN0IHdpZHRoPSIyNDAiIGhlaWdodD0iMzUwIiBmaWxsPSIjRkZGRkZGIiBzdHJva2U9IiNFMEUwRTAiLz4KPHN2ZyB4PSI2MCIgeT0iMTAwIiB3aWR0aD0iMTIwIiBoZWlnaHQ9IjE1MCI+CjxyZWN0IHdpZHRoPSIxMjAiIGhlaWdodD0iMTUwIiBmaWxsPSIjRjVGNUY1Ii8+CjxwYXRoIGQ9Ik0zMCA3NUw5MCA3NUw5MCA5MEwzMCA5MFoiIGZpbGw9IiNEMEQwRDAiLz4KPHN2ZyB4PSI0NSIgeT0iNjAiIHdpZHRoPSIzMCIgaGVpZ2h0PSIzMCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9IiM5MDkwOTAiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIiBzdHJva2UtbGluZWpvaW49InJvdW5kIj4KPHA+CjxwYXRoIGQ9Ik00IDEyaDEybTAtNGgtNm0wIDhIMCIvPgo8L3A+CjwvcGF0aD4KPC9zdmc+Cjwvc3ZnPgo8L3N2Zz4K'
    }
  },
  methods: {
    togglePlay() {
      this.isPlaying = !this.isPlaying;
      // Hier würde die tatsächliche Audio-Logik implementiert
      console.log(this.isPlaying ? 'Playing' : 'Paused');
    },
    seekAudio(value) {
      this.currentTime = value;
      // Hier würde die Audio-Seek-Logik implementiert
      console.log('Seeking to:', value);
    },
    formatTime(seconds) {
      const mins = Math.floor(seconds / 60);
      const secs = Math.floor(seconds % 60);
      return `${mins}:${secs.toString().padStart(2, '0')}`;
    }
  },
  mounted() {
    // Simuliere Audio-Fortschritt
    setInterval(() => {
      if (this.isPlaying && this.currentTime < this.duration) {
        this.currentTime += 1;
      }
    }, 1000);
  }
}
</script>

<style scoped>
.full-height-container {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
 
}

.book-card {
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
}

.book-cover {
  box-shadow: 0 8px 32px rgba(58, 58, 58, 0.1);
  transition: transform 0.3s ease;
}

.book-cover:hover {
  transform: translateY(-4px);
}

.book-image {
  border-radius: 8px;
}

.audio-player-card {
  backdrop-filter: blur(10px);
  background: rgba(255, 255, 255, 0.8) !important;
}

.placeholder-card {
  transition: all 0.3s ease;
  border: 2px dashed #e0e0e0;
}

.placeholder-card:hover {
  border-color: #2196f3;
  background-color: #f3f9ff !important;
}

.placeholder-card-large {
  transition: all 0.3s ease;
  border: 2px dashed #e0e0e0;
}

.placeholder-card-large:hover {
  border-color: #2196f3;
  background-color: #f3f9ff !important;
}

/* Responsive Anpassungen */
@media (max-width: 960px) {
  .book-cover {
    width: 200px !important;
    height: 280px !important;
  }
}

@media (max-width: 600px) {
  .book-cover {
    width: 160px !important;
    height: 240px !important;
  }
}
</style>