<template>
  <div class="list_info">
    <div class="title_info">
      <span class="span">List {{ list.id + 1 }}</span>
      <button
        v-if="isButton(list.list)"
        type="button"
        class="button"
        @click="clickButton"
      >
        {{ infoButton ? "Отсортировать" : "Перемешать" }}
      </button>
    </div>
    <info-item
      @clickElem="clickElem"
      v-if="infoButton"
      :item="stirArray(list.list)"
    />
    <!--
      Не сосвсем понял про сортировку, если она должна быть по колличеству квадратиков то тогда вот так:
      <info-item
      v-else
      @clickElem="clickElem"
      v-for="elem in list.list
        .filter((i) => i.checked && i.count)
        .sort((a, b) => a.count - b.count)"
      :key="list.id + '-' + elem.id"
      :item="sortArray(elem)"
    />
     -->
    <info-item
      v-else
      @clickElem="clickElem"
      v-for="elem in list.list.filter((i) => i.checked && i.count)"
      :key="list.id + '-' + elem.id"
      :item="sortArray(elem)"
    />
  </div>
</template>
<script>
import InfoItem from "./InfoItem.vue";
export default {
  components: {
    InfoItem,
  },
  props: {
    list: { type: Object, required: true },
  },
  data() {
    return {
      infoButton: false,
    };
  },
  methods: {
    clickElem(id) {
      this.$emit("clickElem", this.list.id, id);
    },
    isButton(list) {
      return list.filter((i) => i.checked && i.count).length;
    },
    clickButton() {
      this.infoButton = !this.infoButton;
    },
    stirArray(list) {
      return list
        .filter((i) => i.checked)
        .reduce((acc, elem) => {
          for (let i = 0; i < elem.count; i++) {
            acc.push({ idItem: elem.id, id: i, color: elem.color });
          }
          return acc;
        }, [])
        .sort(() => Math.random() - 0.5);
    },
    sortArray(elem) {
      let res = [];
      for (let i = 0; i < elem.count; i++) {
        res.push({ idItem: elem.id, id: i, color: elem.color });
      }
      return res;
    },
  },
};
</script>

<style scoped>
.list_info {
  overflow: auto;
  border: 2px solid gray;
  padding: 5px;
  margin-bottom: 10px;
}

.title_info {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 5px;
}
.button {
  background: #1da1f2;
  border: none;
  border-radius: 4px;
  color: white;
  font-size: 12px;
  padding: 5px;
  cursor: pointer;
}
</style>
