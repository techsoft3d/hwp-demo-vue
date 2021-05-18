<template>
  <div class="list-group">
    <div></div>
    <ModelTreeItemComponent :hwv="hwv" :node-id="rootNode" :level="0" />
  </div>
</template>

<script lang="ts">
import { defineComponent, provide, ref } from "vue";
import ModelTreeItemComponent from "./model-tree-item-component.vue";

export default defineComponent({
  name: "ModelTreeComponent",
  components: {
    ModelTreeItemComponent,
  },
  setup() {
    // Provide the list of selected nodes to all the children
    const selectedNodeIds = ref(Array<Communicator.NodeId>());
    provide("selectedNodeIds", selectedNodeIds);

    return { selectedNodeIds };
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
        this.selectedNodeIds = [];
        selectionEvents.forEach((event) => {
          const nodeId = event.getSelection().getNodeId();
          if (nodeId) {
            this.selectedNodeIds.push(nodeId);
          }
        });
      },
    });
  },
});
</script>
