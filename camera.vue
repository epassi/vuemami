<template>
  <video id="player" autoplay></video>
</template>

<script>
export default {
  name: "camera",

  props: {
    enabled: {
      type: Boolean,
      default: false
    },
    view: {
      type: String,
      default: "rear",
      validator(value) {
        return ["front", "rear"].indexOf(value) !== -1;
      }
    }
  },

  computed: {
    facingMode() {
      return this.view === "front" ? "front" : "environment";
    }
  },

  watch: {
    enabled() {
      this.enabled ? this.startStream() : this.stopStream();
    },
    view() {
      // Update camera view.
    }
  },

  methods: {
    startStream() {
      // Credit:
      // https://developers.google.com/web/fundamentals/media/capturing-images/
      const player = document.getElementById("player");
      const constraints = {
        video: { facingMode: this.facingMode }
      };
      navigator.mediaDevices.getUserMedia(constraints).then(stream => {
        player.srcObject = stream;
      });
    },

    stopStream() {
      // Credit:
      // https://developers.google.com/web/fundamentals/media/capturing-images/
      const player = document.getElementById("player");
      if (player.srcObject) {
        player.srcObject.getVideoTracks().forEach(track => track.stop());
      }
    },

    recordVideo() {
      // this.$emit("video");
    },

    captureImage() {
      // this.$emit("image");
    }
  },
  mounted() {
    // this.toggleStream();
    this.enabled ? this.startStream() : this.stopStream();
  }
};
</script>

<style lang="scss" scoped>
#player {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
}
</style>
