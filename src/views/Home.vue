<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-card :loading="loadingAudio">
          <v-card-title>Task 1</v-card-title>
          <v-card-text>
            <wave-surfer
              @error="errorMessage = $event"
              :loading.sync="loadingAudio"
              :playing.sync="isPlaying"
              :options="options"
              ref="player"
            ></wave-surfer>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn @click="playPause" icon outlined>
              <v-icon>{{ isPlaying ? "mdi-pause" : "mdi-play" }}</v-icon>
            </v-btn>
            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="4">
        <v-card>
          <v-card-title>Segments</v-card-title>
          <v-card-text> All my segments go here. </v-card-text>
        </v-card>
      </v-col>
      <v-col cols="8">
        <v-card>
          <v-card-title>Data</v-card-title>
          <v-card-text>All annotation data goes here.</v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import Vue from "vue";
import WaveSurfer from "@/components/WaveSurfer.vue";
export default Vue.extend({
  name: "Home",
  components: {
    WaveSurfer,
  },
  data() {
    return {
      loadingAudio: false,
      isPlaying: false,
      errorMessage: null,
      options: { file: require("../assets/test.mp3"), scrollParent: true },
    };
  },
  computed: {
    // eslint-disable-next-line
    player(): any {
      return this.$refs.player;
    },
  },
  methods: {
    playPause() {
      this.player.playPause();
    },
  },
});
</script>
