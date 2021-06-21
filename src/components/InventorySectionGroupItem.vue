<template>
  <div class="inventory-section-group-item-component">
    <p>{{ itemDataProps.itemSectionCategoryItemName }}</p>
    <p>{{ itemDataProps.itemSectionCategoryItemStatus }}</p>
  </div>
</template>

<script>
export default {
  name: "InventorySectionGroupItem",
  props: {
    itemDataProps: {
      type: Object,
      required: true,
    },
  },
  mounted() {},
  computed: {},
  // data() {
  //   return {
  //     itemData: JSON.parse(JSON.stringify(this.props.itemDataProps))
  //   };
  // },
};
</script>

<style scoped></style>
