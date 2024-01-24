<template>
  <div>
    <BaseAccordion :items="accordionItems" @toggleItem="toggleAccordionItem">
      <template v-slot:title="{ item, index }">
        <input
          type="checkbox"
          @click.stop
          @input="handleCheckTree(!item.isChecked, index)"
          v-model="item.isChecked"
        />
        <strong>{{ item.title }}</strong>
      </template>
      <template v-slot:content="{ item, index }">
        <p v-for="subitem in item.items" :key="subitem.title">
          <input
            type="checkbox"
            v-model="subitem.isChecked"
            @change="handleTopCheckbox(index)"
          />
          <strong>{{ subitem.title }}</strong>
          <BaseInput v-model="subitem.count" :initialValue="subitem.count" label="Count" />
          <BaseInput v-model="subitem.color" :initialValue="subitem.color" label="Color" />
        </p>
      </template>
    </BaseAccordion>
    <div class="boxes">
      <div
        class="boxes__row"
        v-for="list in accordionItems"
        v-show="list.isChecked"
        :key="list.title"
      >
        <div
          class="boxes__box-group"
          v-for="box in list.items"
          v-show="box.isChecked"
          :key="box.title"
        >
          <div
            v-for="idx in +box.count"
            :key="idx"
            class="boxes__box"
            :style="{ 'background-color': box.color }"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import BaseAccordion from './components/BaseAccordion.vue';
import BaseInput from './components/BaseInput.vue';

export default {
  components: {
    BaseAccordion,
    BaseInput,
  },
  data() {
    return {
      isChecked: false,
      inputText: '',
      accordionItems: [
        {
          title: 'Tree first',
          isChecked: false,
          isOpen: false,
          items: [
            { title: 'Tree first - Item first', isChecked: false, color: 'red', count: 1 },
            { title: 'Tree first - Item second', isChecked: false, color: 'blue', count: 5 },
            { title: 'Tree first - Item third', isChecked: false, color: 'green', count: 10 },
          ]
        },
        {
          title: 'Tree second',
          isChecked: false,
          isOpen: false,
          items: [
            { title: 'Tree second - Item first', isChecked: false, color: 'purple', count: 1 },
            { title: 'Tree second - Item second', isChecked: false, color: 'orange', count: 5 },
            { title: 'Tree second - Item third', isChecked: false, color: 'pink', count: 10 },
          ]
        },
      ],
    };
  },
  methods: {
    handleCheckTree(isChecked, itemIndex) {
      this.accordionItems[itemIndex].items.forEach(item => {
        item.isChecked = isChecked
      })
    },
    handleTopCheckbox(idx) {
      const isAllChecked = this.accordionItems[idx].items.every(item => item.isChecked)
      const isAllNotChecked = this.accordionItems[idx].items.every(item => !item.isChecked)

      if (isAllChecked) {
        this.accordionItems[idx].isChecked = true
      } else if (isAllNotChecked) {
        this.accordionItems[idx].isChecked = false
      }
    },
    toggleAccordionItem(index) {
      const { isOpen } = this.accordionItems[index]

      this.accordionItems[index].isOpen = !isOpen
    }
  }
};
</script>

<style>
.boxes {
  padding: 20px 0;
  display: flex;
  flex-direction: column;
}
.boxes__row {
  display: flex;
  flex-direction: row;
  padding: 20px 0;
}
.boxes__box-group {
  display: flex;
  flex-direction: row;
}
.boxes__box {
  height: 10px;
  width: 10px;
  border-radius: 2px;
  content: '';
  margin: 2px;
}
</style>