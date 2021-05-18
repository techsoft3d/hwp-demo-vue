<template>
  <div class="list-group">
    <ModelTreeItemComponent
      :hwv="hwv"
      :node-id="getRootNode(hwv)"
      :level="0"
    ></ModelTreeItemComponent>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import ModelTreeItemComponent from "./model-tree-item-component.vue";

export default defineComponent({
  components: {
    ModelTreeItemComponent,
  },
  props: {
    hwv: {
      type: Communicator.WebViewer,
      required: true,
    },
  },
  data() {
    return {
      itemList: [],
    };
  },
  methods: {
    getRootNode(webViewer: Communicator.WebViewer) {
      return webViewer.model.getAbsoluteRootNode();
    },
  },
});
</script>
