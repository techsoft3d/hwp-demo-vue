<template>
  <!-- container -->
  <div class="container-fluid p-0 m-0">
    <div class="row p-0 m-0" v-bind:style="{ height: '100vh' }">
      <!-- HWP WebViewer with Custom Component -->
      <div class="col-6 p-0 m-0 border-end">
        <ViewerComponent v-bind:model-uri="modelPath" @hwv-ready="hwvReady"></ViewerComponent>
      </div>
      <!-- Control panel on the right side -->
      <div
        class="col-6 p-0 m-0 overflow-scroll"
        v-bind:style="{ height: '100vh' }"
      >
        <!-- Logo -->
        <img
          v-bind:src="imgPath"
          class="img-fluid m-3"
          v-bind:style="{ maxHeight: '100px' }"
          alt="TechSoft 3D LOGO"
        />
        <!-- NavBar -->
        <h1>New H1</h1>
        <ul class="nav nav-tabs px-3">
        </ul>
        <!-- Tab Contents -->
        <div class="tab-content p-3">
        </div>
        <!-- Tab Contents End -->
      </div>
      <!-- Right Panel End -->
    </div>
    <!-- Row End -->
  </div>
</template>

<script lang="ts">
import ViewerComponent from "./components/viewer-component.vue";
import { ref, defineComponent } from 'vue'

export default defineComponent({
  name: "App",
  components: {
    ViewerComponent,
  },
  data() {
    return {
      // Attach base url so the path will always work
      modelPath: process.env.BASE_URL + "assets/microengine.scs",
      imgPath: process.env.BASE_URL + "assets/ts3d_logo.png",
      // cameraStatus: ref<CameraStatus | undefined>(undefined),
      cameraStatus: Object(),
      isStructureReady: false,
    };
  },
  methods: {
    hwvReady(hwv: Communicator.WebViewer): void {
      hwv.setCallbacks({
        sceneReady: () => {
          this.cameraStatus = hwv.view.getCamera().toJson();
        },
        modelStructureReady: () => {
          this.isStructureReady = true;
        },
        camera: () => {
          this.cameraStatus = hwv.view.getCamera().toJson();
        },
      });
    },
  },
});
</script>

<style>
</style>
