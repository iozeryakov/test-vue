<template>
  <ul class="list">
    <div class="title" @click="OpenClose">
      <div class="arrow" :class="isOpen && 'arrow135'"></div>
      <input
        type="checkbox"
        @change="$emit('changeList', $event.target.checked, list.id)"
        v-model="list.checked"
        :id="`check${list.id}`"
        class="none"
      />
      <label
        @click.stop="OpenClose"
        :for="`check${list.id}`"
        class="check item_check"
        :class="isPoint(list.list) && 'point_check'"
      ></label>
      <span class="span">List {{ list.id + 1 }}</span>
    </div>
    <div v-if="isOpen">
      <item
        @changeItem="$emit('changeItem', list.id)"
        v-for="elem in list.list"
        :key="elem.id"
        :id="`${list.id}`"
        :item="elem"
        @update:value="validate"
      />
    </div>
  </ul>
</template>
<script>
import Item from "./Item.vue";
export default {
  components: {
    Item,
  },
  props: {
    list: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      isOpen: false,
    };
  },
  methods: {
    validate(elem) {
      this.$emit("update:value", {
        ...this.list,
        list: this.list.list.map((i) =>
          i.id === elem.id ? { ...elem } : { ...i }
        ),
      });
    },
    OpenClose() {
      this.isOpen = !this.isOpen;
    },
    isPoint(list) {
      return list.filter((i) => i.checked).length > 0;
    },
  },
};
</script>

<style scoped>
.list {
  display: flex;
  flex-direction: column;
  list-style-type: none;
}
.list:not(:first-child) {
  margin-top: 10px;
}
.arrow {
  margin-right: 10px;
  width: 10px;
  height: 10px;
  border-top: 2px solid gray !important;
  border-right: 2px solid gray !important;
  transform: rotate(45deg);
  display: inline-block;
  transition: 0.3s;
}
.arrow135 {
  transform: rotate(135deg);
}

.title {
  align-items: center;
  display: flex;
  width: min-content;
  cursor: pointer;
}
</style>
