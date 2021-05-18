<template>
  <div
    :style="{ paddingLeft: level * 20 + 10 + 'px', marginBottom: -1 + 'px' }"
    class="list-group-item list-group-item-action d-flex py-1"
  >
    <div class="py-1"></div>
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
  <!-- <app-model-tree-item *ngFor="let child of children" [ngClass]="{'d-none': isCollapsed}" [modelTree]="modelTree" [hwv]="hwv" [nodeId]="child" [level]="level+1"></app-model-tree-item> -->
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "ModelTreeItemComponent",
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
      isSelected: false,
    };
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
