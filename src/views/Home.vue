<template>
  <v-card :loading="loadingAudio">
    <v-card-title>Task 1</v-card-title>
    <v-card-text>
      <wave-surfer
        @error="errorMessage = $event"
        :loading.sync="loadingAudio"
        :options="options"
        ref="player"
      ></wave-surfer>
    </v-card-text>
    <v-card-actions>
      <v-btn @click="play" icon outlined><v-icon>mdi-play</v-icon></v-btn>
      <v-btn @click="pause" icon outlined><v-icon>mdi-pause</v-icon></v-btn>
    </v-card-actions>
  </v-card>
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
      errorMessage: null,
      options: { file: require("../assets/test.mp3") },
    };
  },
  computed: {
    player(): any {
      return this.$refs.player!;
    },
  },
  methods: {
    play() {
      this.player.play();
    },
    pause() {
      this.player.pause();
    },
  },
});
</script>
