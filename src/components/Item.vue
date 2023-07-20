<template>
  <li class="item">
    <div class="item_wrapper">
      <input
        type="checkbox"
        class="none"
        @change="$emit('changeItem')"
        :id="`check${id}-${item.id}`"
        v-model="item.checked"
      />
      <label :for="`check${id}-${item.id}`" class="check item_check"></label>
      <span class="span">Item {{ item.id + 1 }}</span>
    </div>
    <div class="item_wrapper">
      <input
        type="number"
        class="number"
        :value="item.count"
        @input="validate"
      />
      <input type="color" class="color" v-model="item.color" />
    </div>
  </li>
</template>
<script>
export default {
  data() {
    return {};
  },
  props: {
    id: {
      type: String,
      required: true,
    },
    item: {
      type: Object,
      required: true,
    },
  },
  methods: {
    validate(event) {
      this.$emit("update:value", {
        ...this.item,
        count: event.target.value < 0 ? 0 : Number(event.target.value),
      });
    },
  },
};
</script>
<style scoped>
.item {
  justify-content: space-between;
  display: flex;
  margin: 0 50px;
}
.item_wrapper {
  margin-top: 10px;
  align-items: center;
  display: flex;
}
.number::-webkit-outer-spin-button,
.number::-webkit-inner-spin-button {
  -webkit-appearance: none;
}
.number:focus {
  outline: 1px solid gray;
}
.number {
  -moz-appearance: textfield;
  margin-right: 10px;
  width: 25px;
  border: 0;
}
.color:focus {
  outline: none;
}

.color {
  -webkit-appearance: none;
  padding: 0;
  border: 0;
  cursor: pointer;
  width: 18px;
  height: 18px;
}
.color::-webkit-color-swatch-wrapper {
  padding: 0;
}

.color::-webkit-color-swatch {
  border: none;
}
.color::-moz-focus-inner {
  border: none;
  padding: 0;
}
.color::-moz-color-swatch {
  border: none;
}
</style>
