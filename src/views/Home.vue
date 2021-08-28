<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-card :loading="loadingAudio">
          <v-card-title>Task 1</v-card-title>
          <v-divider></v-divider>
          <v-card-text>
            <div id="wave-timeline"></div>
            <wave-surfer
              @ready="onSurferReady"
              @error="errorMessage = $event"
              :loading.sync="loadingAudio"
              :playing.sync="isPlaying"
              :options="options"
              ref="player"
            ></wave-surfer>
          </v-card-text>
          <v-divider></v-divider>
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
import Surfer from "@/components/Surfer.vue";
import Timeline from "wavesurfer.js/dist/plugin/wavesurfer.timeline.js";
import Regions from "wavesurfer.js/dist/plugin/wavesurfer.regions.js";
import Minimap from "wavesurfer.js/dist/plugin/wavesurfer.minimap.js";
import colors from "vuetify/lib/util/colors";
export default Vue.extend({
  name: "Home",
  components: {
    WaveSurfer: Surfer,
  },
  data() {
    return {
      loadingAudio: false,
      isPlaying: false,
      errorMessage: null,
      options: {
        file: require("../assets/test.mp3"),
        scrollParent: true,
        normalize: true,
        minimap: true,
        progressColor: colors.blue.darken1,
        waveColor: colors.grey.base,
        plugins: [
          Regions.create(),
          Minimap.create({
            height: 30,
            waveColor: colors.grey.lighten3,
            progressColor: colors.grey.base,
            cursorColor: colors.grey.base,
          }),
          Timeline.create({
            container: "#wave-timeline",
          }),
        ],
      },
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
    onSurferReady() {
      this.player.instance.enableDragSelection({
        color: "#ffc10766",
      });
      this.player.instance.on("region-click", (region: any, e: MouseEvent) => {
        e.stopPropagation();
        // Play on click, loop on shift click
        e.shiftKey ? region.playLoop() : region.play();
      });
      this.player.instance.on("region-click", this.editAnnotation);
      this.player.instance.on("region-updated", this.saveRegions);
      this.player.instance.on("region-removed", this.saveRegions);
      this.player.instance.on("region-in", this.showNote);

      this.player.instance.on("region-play", (region: any) => {
        region.once("out", (region: any) => {
          this.player.instance.play(region.start);
          this.player.instance.pause();
        });
      });
    },
    editAnnotation(region: any) {
      console.log(region);
    },
    showNote(region: any) {
      console.log(region);
    },
    saveRegions() {
      localStorage.regions = JSON.stringify(
        Object.keys(this.player.instance.regions.list).map((id) => {
          let region = this.player.instance.regions.list[id];
          return {
            start: region.start,
            end: region.end,
            attributes: region.attributes,
            data: region.data,
          };
        })
      );
    },
  },
});
</script>
