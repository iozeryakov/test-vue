<template>
  <div class="info">
    <div
      class="item_info"
      @click="$emit('clickElem', elem.idItem)"
      v-for="elem in item"
      :key="elem.idItem + '-' + elem.id"
      :style="{ background: elem.color }"
    ></div>
  </div>
</template>
<script>
export default {
  props: {
    item: {
      type: Array,
      required: true,
    },
  },
};
</script>
<style scoped>
.item_info {
  min-width: 10px;
  min-height: 10px;
  margin: 2px;
  background: black;
}
.info {
  overflow: hidden;
  flex-wrap: wrap;
  display: flex;
  margin-bottom: 5px;
}
</style>
