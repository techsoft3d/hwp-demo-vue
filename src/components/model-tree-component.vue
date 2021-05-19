<template>
  <div class="list-group">
    <div></div>
    <ModelTreeItemComponent :hwv="hwv" :node-id="rootNode" :level="0" />
  </div>
</template>

<script lang="ts">
import { defineComponent, provide, reactive } from "vue";
import ModelTreeItemComponent from "./model-tree-item-component.vue";

export default defineComponent({
  name: "ModelTreeComponent",
  components: {
    ModelTreeItemComponent,
  },
  setup() {
    // Provide the state of selected nodes to all the children
    const nodeSelectionStates = reactive<{[key:number]:boolean}>({});
    provide("nodeSelectionStates", nodeSelectionStates);

    return { nodeSelectionStates };
  },
  props: {
    hwv: {
      type: Communicator.WebViewer,
      required: true,
    },
  },
  computed: {
    rootNode(): number {
      return this.hwv.model.getAbsoluteRootNode();
    },
  },
  created() {
    this.hwv.setCallbacks({
      selectionArray: (selectionEvents) => {
        // Update selected node id's
        for (var key in this.nodeSelectionStates) {
          this.nodeSelectionStates[key] = false;
        }
        selectionEvents.forEach((event) => {
          const nodeId = event.getSelection().getNodeId();
          if (nodeId != null) {
            this.nodeSelectionStates[nodeId] = true;
          }
        });
      },
    });
  },
});
</script>
