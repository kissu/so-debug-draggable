<template>
  <div class="inventory-section">    
    <draggable v-model="itemSectionProps.itemSectionCategory">
      <transition-group>
        <div
          v-for="category in itemSectionProps.itemSectionCategory"
          :key="category.itemSectionCategoryId"
        >
          <!-- <p>{{ category.itemSectionCategoryName }}</p>  -->
          <inventory-section-group :itemSectionGroupProps="category">
          </inventory-section-group>
        </div>
      </transition-group>
    </draggable>
  </div>
</template>

<script>
import InventorySectionGroup from "./InventorySectionGroup";
import draggable from "vuedraggable";

export default {
  name: "Inventory",
  components: {
    InventorySectionGroup,
    draggable,
  },
  props: {
    itemSectionProps: {
      type: Object,
      required: true,
      default() {
        return { itemSectionCategory: {} };
      },
    },
  },
  mounted() {},
  created() {},
  computed: {},
  data() {
    return {
      itemSectionData: JSON.parse(JSON.stringify(this.props.itemSectionProps))
    };
  },  
};
</script>

<style scoped></style>
