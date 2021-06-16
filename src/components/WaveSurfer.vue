<template>
  <div class="wavesurfer-vue"></div>
</template>

<script>
import Vue from "vue";
import WaveSurfer from "wavesurfer.js";

export default Vue.extend({
  name: "WaveSurfer",
  props: {
    options: {
      type: Object,
      required: true,
    },
  },
  data() {
    return { instance: null, loading: false };
  },
  mounted() {
    const el = this.$el;
    this.instance = WaveSurfer.create({
      container: el,
      ...this.options,
    });
    this.attachHandlers();
    this.instance.load(this.options.file);
  },
  beforeDestroy() {
    this.instance.destroy();
  },
  methods: {
    syncLoadingStatus() {
      this.$emit("update:loading", this.loading);
    },
    play() {
      this.instance.play();
    },
    pause() {
      this.instance.pause();
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
    },
  },
});
</script>

<style scoped>
.wavesurfer-vue {
  position: relative;
}
</style>
