<template>
  <div>
    <div v-for="(item, index) in items" :key="index">
      <div @click="toggleItem(index)" class="accordion-item">
        <slot name="title" :item="item" :index="index" />
      </div>
      <div v-show="item.isOpen" class="accordion-content">
        <slot name="content" :item="item" :index="index" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: Array,
  },
  emits: ['toggleItem'],
  setup(props, ctx) {
    const toggleItem = (index) => {
      ctx.emit('toggleItem', index)
    };

    return {
      toggleItem,
    };
  },
};
</script>

<style scoped>
.accordion-item {
  cursor: pointer;
  padding: 10px;
  background-color: #f0f0f0;
  margin-bottom: 5px;
}

.accordion-content {
  padding: 10px;
  background-color: #e0e0e0;
}
</style>