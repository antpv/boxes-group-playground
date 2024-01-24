<template>
  <div>
    <label>
      {{ label }}
      <input type="text" :value="inputValue" @input="updateInputValue" />
    </label>
  </div>
</template>

<script>
import { ref, watch } from 'vue';

export default {
  props: {
    label: String,
    initialValue: [String, Number],
  },
  setup(props, { emit }) {
    const inputValue = ref(props.initialValue || '');

    watch(inputValue, (newValue) => {
      emit('update:modelValue', newValue);
    });

    const updateInputValue = (event) => {
      inputValue.value = event.target.value;
    };

    return {
      inputValue,
      updateInputValue,
    };
  },
};
</script>