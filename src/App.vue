<template>
  <!-- container -->
  <div class="container-fluid p-0 m-0">
    <div class="row p-0 m-0" v-bind:style="{ height: '100vh' }">
      <!-- HWP WebViewer with Custom Component -->
      <div class="col-6 p-0 m-0 border-end">
        <ViewerComponent
          v-bind:model-uri="modelPath"
          @hwv-ready="hwvReady"
        ></ViewerComponent>
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
        <ul class="nav nav-tabs px-3">
          <li className="nav-item">
            <button
              class="nav-link text-dark"
              :class="{ active: currentTab == 1 }"
              @click="changeTab(1)"
              type="button"
            >
              Home
            </button>
          </li>
          <li className="nav-item">
            <button
              class="nav-link text-dark"
              :class="{ active: currentTab == 2 }"
              @click="changeTab(2)"
              type="button"
            >
              Model Structure
            </button>
          </li>
        </ul>
        <!-- Tab Contents -->
        <div class="tab-content p-3">
          <!-- Home Tab -->
          <div
            class="tab-pane fade show"
            :class="{ active: currentTab == 1 }"
          >
            <h2>Vue.js Demo for Hoops Web Platform</h2>
            <!-- Operator Selection -->
            <h5>Operator</h5>
            <select class="form-select mb-3">
              <option value="Orbit">Orbit</option>
              <option value="Area Select">Area Select</option>
              <option value="Select">Select</option>
              <option value="Measure">Measure</option>
            </select>
            <!-- Camera Status -->
            <h5>Camera Status</h5>
            <p v-if="!cameraStatus">unavailable</p>
            <div v-else>
              <p class="mb-0">
                <strong>Position:</strong> ({{
                  cameraStatus.position.x.toFixed(2)
                }}, {{ cameraStatus.position.y.toFixed(2) }},
                {{ cameraStatus.position.z.toFixed(2) }})
              </p>
              <p class="mb-0">
                <strong>Target:</strong> ({{
                  cameraStatus.target.x.toFixed(2)
                }}, {{ cameraStatus.target.y.toFixed(2) }},
                {{ cameraStatus.target.z.toFixed(2) }})
              </p>
              <p class="mb-0">
                <strong>Up:</strong> ({{ cameraStatus.up.x.toFixed(2) }},
                {{ cameraStatus.up.y.toFixed(2) }},
                {{ cameraStatus.up.z.toFixed(2) }})
              </p>
              <p class="mb-0">
                <strong>Width:</strong>
                {{ cameraStatus.width.toFixed(2) }} &nbsp;
                <strong>Height:</strong> {{ cameraStatus.height.toFixed(2) }}
              </p>
              <p class="mb-0">
                <strong>Projection:</strong>
                {{ cameraStatus.projection.toFixed(2) }} &nbsp;
                <strong>NearLimit:</strong>
                {{ cameraStatus.nearLimit.toFixed(2) }}
              </p>
              <p class="mb-0">
                <strong>Class Name:</strong> {{ cameraStatus.className }}
              </p>
            </div>
          </div>
          <!-- Home Tab End -->
          <!-- ModelStructure Tab -->
          <div
            class="tab-pane fade show"
            :class="{ active: currentTab == 2 }"
          >
            <h5>Model Structure</h5>
            <p v-if="!isStructureReady">Model structure is not ready"</p>
            <!-- <app-model-tree v-else
              *ngIf="modelStructureIsReady"
              [hwv]="hwv"
            ></app-model-tree> -->
          </div>
          <!-- ModelStructure Tab End -->
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
import { defineComponent } from "vue";
import { CameraStatus } from "./data/camera-status";

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
      cameraStatus: undefined as CameraStatus | undefined,
      isStructureReady: false,
      currentTab: 1, // 1: Home, 2: ModelTree
      operator: "Orbit",
    };
  },
  methods: {
    // Callback for when the web viewer is ready
    hwvReady(hwv: Communicator.WebViewer): void {
      hwv.setCallbacks({
        sceneReady: () => {
          this.cameraStatus = hwv.view.getCamera().toJson() as CameraStatus;
        },
        modelStructureReady: () => {
          this.isStructureReady = true;
        },
        camera: () => {
          this.cameraStatus = hwv.view.getCamera().toJson() as CameraStatus;
        },
      });
    },
    changeTab(newTab: number) {
      this.currentTab = newTab;
    },
  },
});
</script>

<style>
</style>
