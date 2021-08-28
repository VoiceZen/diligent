<template>
  <div class="wavesurfer-vue"></div>
</template>

<script>
import Vue from "vue";
import WaveSurfer from "wavesurfer.js";

export default Vue.extend({
  name: "WaveSurfer",
  props: {
    // All the options that WaveSurfer supports.
    // Check https://wavesurfer-js.org/docs/options.html
    options: {
      type: Object,
      required: true,
    },
  },
  data() {
    return { instance: null, loading: false };
  },
  mounted() {
    // We want wavesurfer to find the div that THIS component
    // creates and then attach to it. Giving a class or id will
    // create problems when there are multiple instances together.
    const el = this.$el;
    this.instance = WaveSurfer.create({
      container: el,
      ...this.options,
    });
    this.attachHandlers();
    this.instance.load(this.options.file);
  },
  beforeDestroy() {
    // Destroy all the stuff created by wavesurfer
    this.instance.destroy();
  },
  computed: {
    isPlaying() {
      return this.instance?.isPlaying() ?? false;
    },
  },
  methods: {
    /**
     * Sends current loading status back to parent component.
     * This will be available in parent via :loading.sync="parentVariable".
     */
    syncLoadingStatus() {
      this.$emit("update:loading", this.loading);
    },
    attachHandlers() {
      this.instance.on("loading", () => {
        this.loading = true;
        this.syncLoadingStatus();
      });
      this.instance.on("ready", () => {
        this.$emit("ready");
        this.loading = false;
        this.syncLoadingStatus();
      });
      this.instance.on("error", (errorMessage) => {
        this.$emit("error", errorMessage);
        this.loading = false;
        this.syncLoadingStatus();
      });
      this.instance.on("play", () => {
        this.$emit("update:playing", true);
      });
      this.instance.on("pause", () => {
        this.$emit("update:playing", false);
      });
    },
    // Convenience methods so the parent doesn't have to
    // get hold of the wavesurfer instance for normal
    // use cases. For advanced usage, the consumer of this
    // component is encouraged to use the instance itself.
    play(start, end) {
      this.instance.play(start, end);
    },
    pause() {
      this.instance.pause();
    },
    playPause() {
      this.instance.playPause();
    },
  },
});
</script>
