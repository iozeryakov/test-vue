<template>
  <div class="wrapper">
    <div class="block">
      <list
        v-for="elem in Lists"
        :key="elem.id"
        :list="elem"
        @changeList="changeList"
        @changeItem="changeItem"
        @update:value="validate"
      />
    </div>
    <div class="block">
      <info-list
        @clickElem="clickElem"
        v-for="elem in Lists"
        :key="elem.id"
        :list="elem"
      />
    </div>
  </div>
</template>

<script>
import List from "./components/List.vue";
import InfoList from "./components/InfoList.vue";
export default {
  components: {
    List,
    InfoList,
  },
  data() {
    return {
      Lists: [],
    };
  },
  methods: {
    validate(elem) {
      this.Lists = this.Lists.map((el) =>
        el.id === elem.id ? { ...elem } : { ...el }
      );
    },
    clickElem(idList, idItem) {
      this.Lists = this.Lists.map((el) =>
        el.id === idList
          ? {
              ...el,
              list: el.list.map((i) =>
                i.id === idItem ? { ...i, count: i.count - 1 } : { ...i }
              ),
            }
          : { ...el }
      );
    },
    changeItem(id) {
      this.Lists = this.Lists.map((item) =>
        item.id === id
          ? {
              ...item,
              checked:
                item.list.filter((i) => i.checked === true).length ===
                item.list.length
                  ? true
                  : false,
            }
          : { ...item }
      );
    },
    changeList(checked, id) {
      this.Lists = this.Lists.map((item) =>
        item.id === id
          ? {
              ...item,
              list: item.list.map((el) => ({ ...el, checked: checked })),
            }
          : { ...item }
      );
    },
    createList() {
      for (let i = 0; i < 5; i++) {
        let list = [];
        for (let j = 0; j < Math.floor(Math.random() * 7 + 4); j++) {
          let letters = "0123456789ABCDEF";
          let color = "#";
          for (let i = 0; i < 6; i++) {
            color += letters[Math.floor(Math.random() * 16)];
          }

          list.push({
            id: j,
            checked: false,
            count: Math.floor(Math.random() * 101),
            color: color,
          });
        }
        this.Lists.push({
          id: i,
          checked: false,
          list: list,
        });
      }
    },
  },
  mounted() {
    this.createList();
  },
};
</script>

<style>
.span {
  white-space: nowrap;
  margin-right: 5px;
}
.none {
  display: none;
}
.check {
  margin-right: 10px;
  position: relative;
  cursor: pointer;
  display: flex;
  max-width: min-content;
  padding: 8px;
  border: 1px solid gray;
  max-width: 18px;
  max-height: 18px;
}
.none:not(:checked) + .point_check {
  padding: 5px;
}
.none:checked + .item_check::before {
  position: absolute;
  left: 0;
  bottom: 1px;
  height: 40%;
  width: 1px;
  background-color: black;
  content: "";
  transform: translateX(5px) rotate(-45deg);
  transform-origin: left bottom;
}
.none:checked + .item_check::after {
  position: absolute;
  left: 0;
  bottom: 1px;
  height: 1px;
  width: 100%;
  background-color: black;
  content: "";
  transform: translateX(5px) rotate(-50deg);
  transform-origin: left bottom;
}
.point_check::after {
  content: "";
  background: black;
  width: 6px;
  height: 6px;
}
#app {
  display: flex;
}
* {
  margin: 0;
  padding: 0;
}
.wrapper {
  overflow: hidden;
  margin: 20px;
  display: flex;
  width: 100%;
  justify-content: space-between;
}
.block:first-child {
  margin-right: 10%;
}
.block {
  overflow: auto;
  display: flex;
  flex-direction: column;
  padding: 25px 10px;
  width: 40%;
  max-width: 45%;
  border: 2px solid gray;
}
</style>
