<template>
  <div class="inventory-section-group">
    <p>{{ itemSectionGroupProps.itemSectionCategoryName }}</p>
    <div
      v-for="group in itemSectionGroupProps.itemSectionCategoryItemList"
      :key="group.itemSectionCategoryId"
    >
      <inventory-section-group-item :itemDataProps="group">
      </inventory-section-group-item>
    </div>
    <!-- <draggable v-model="itemSectionGroupProps.itemSectionCategoryItemList">
      <transition-group>
        <div
          v-for="group in itemSectionGroupProps.itemSectionCategoryItemList"
          :key="group.itemSectionCategoryId"
        >
          <inventory-section-group-item :itemDataProps="group">
          </inventory-section-group-item>
        </div>
      </transition-group>
    </draggable> -->
  </div>
</template>

<script>
import InventorySectionGroupItem from "./InventorySectionGroupItem";
// import draggable from "vuedraggable";

export default {
  name: "InventorySectionGroup",
  components: {
    InventorySectionGroupItem,
    // draggable,
  },
  props: {
    itemSectionGroupProps: {
      type: Object,
      required: true,
      default() {
        return { itemSectionCategoryItemList: {} };
      },
    },
  },
  mounted() {},
  computed: {},
  data() {
    return {
      itemSectionGroupData: JSON.parse(
        JSON.stringify(this.props.itemSectionGroupProps)
      ),
    };
  },
};
</script>

<style scoped></style>
