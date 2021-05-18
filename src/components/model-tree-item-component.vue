<template>
  <div
    style="
       {
        margin-bottom: -1px;
      }
    "
    :style="{ paddingLeft: level * 20 + 10 + 'px' }"
    class="list-group-item list-group-item-action d-flex py-1"
    :class="{
      'd-none': nodeName.length <= 0,
      'bg-primary text-white': isSelected,
    }"
  >
    <div
      class="py-1"
      :class="{
        caret: childrenIds.length > 0,
        'caret-down': !isCollapsed,
      }"
    ></div>
    <div class="py-1 flex-fill cursor-pointer user-select-none">
      {{ nodeName }}
    </div>
  </div>
  <ModelTreeItemComponent
    v-for="childId in childrenIds"
    :nodeId="childId"
    :level="level + 1"
    :hwv="hwv"
    :key="childId"
  ></ModelTreeItemComponent>
</template>

<script lang="ts">
import { defineComponent, inject } from "vue";

export default defineComponent({
  name: "ModelTreeItemComponent",
  setup() {
    // Inject Data
    const selectedNodeIds = inject<Communicator.NodeId[]>("selectedNodeIds");
    return { selectedNodeIds };
  },
  emits: {
    itemReady: null,
  },
  props: {
    nodeId: {
      type: Number,
      required: true,
    },
    level: {
      type: Number,
      required: true,
    },
    hwv: {
      type: Communicator.WebViewer,
      required: true,
    },
  },
  data() {
    return {
      nodeName: "",
      childrenIds: [] as number[],
      isCollapsed: false,
    };
  },
  computed: {
    isSelected(): boolean {
      if (!this.selectedNodeIds) {
        return false;
      }
      return this.selectedNodeIds.includes(this.nodeId);
    },
  },
  created() {
    switch (this.hwv.model.getNodeType(this.nodeId)) {
      case Communicator.NodeType.Part:
      case Communicator.NodeType.PartInstance:
      case Communicator.NodeType.BodyInstance:
      case Communicator.NodeType.AssemblyNode: {
        let nName = this.hwv.model.getNodeName(this.nodeId);
        this.nodeName = nName ? nName : "";
        this.childrenIds = this.hwv.model.getNodeChildren(this.nodeId);
        break;
      }
      default:
        break;
    }
  },
  mounted() {
    this.$emit("itemReady", this);
  },
});
</script>

<style scoped>
/* Style the caret/arrow */
.caret {
  cursor: pointer;
  user-select: none; /* Prevent text selection */
}

/* Create the caret/arrow with a unicode, and style it */
.caret::before {
  content: "\25B6";
  /* color: black; */
  display: inline-block;
  margin-right: 6px;
}

/* Rotate the caret/arrow icon when clicked on (using JavaScript) */
.caret-down::before {
  transform: rotate(90deg);
}

.cursor-pointer {
  cursor: pointer;
}
</style>
