<template>
  <div
    v-bind:id="viewerId"
    class="position-relative w-100 h-100 bg-light"
  ></div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { v4 as uuidv4 } from "@uuid";

export default defineComponent({
  name: "ViewerComponent",
  emits: {
    hwvReady: null
  },
  props: {
    modelUri: String,
  },
  data() {
    return {
      viewerId: String(),
    };
  },
  created() {
    // Generate a unique ID for the web viewer DOM element
    this.viewerId = uuidv4();
  },
  mounted() {
    // Create the Web Viewer
    const hwv = new Communicator.WebViewer({
      containerId: this.viewerId,
      endpointUri: this.modelUri,
    });
    hwv.setCallbacks({
      sceneReady: () => {
        hwv.view.setBackgroundColor(
          Communicator.Color.white(),
          Communicator.Color.white()
        );
      },
    });
    hwv.start();
    window.addEventListener("resize", () => {
      hwv.resizeCanvas();
    });

    // Emits an event to tell the parent that the web viewer is ready
    this.$emit("hwvReady", hwv);
  },
});
</script>
