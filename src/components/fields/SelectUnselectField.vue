<template>
  <div class="select-unselect-field">
      <div class="options-container">
          <div class="available-options">
              <h3>Available Options</h3>
              <div v-for="option in availableOptions" :key="option.id" class="option"
                  @click="toggleOption(option.id)">
                  {{ option.label }}
              </div>
          </div>

          <div class="disabled-options">
              <h3>Disabled Options</h3>
              <div v-for="option in disabledOptions" :key="option.id" class="option disabled"
                  @click="toggleOption(option.id)">
                  {{ option.label }}
              </div>
          </div>
      </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import fieldMixin from '../FieldMixin';

const emit = defineEmits(['update']);
const props = defineProps({
  field: {
      type: Object,
      required: true,
  },
  modelValue: {
      type: Object,
      default: () => ({}),
  },
});

const availableOptions = ref(props.field.options.filter(option => !props.modelValue[option.id]));
const disabledOptions = ref(props.field.options.filter(option => props.modelValue[option.id]));

const toggleOption = (optionId) => {
  if (disabledOptions.value.some(option => option.id === optionId)) {
      moveToAvailable(optionId);
  } else {
      moveToDisabled(optionId);
  }
};

const moveToAvailable = (optionId) => {
  const option = disabledOptions.value.find(o => o.id === optionId);
  disabledOptions.value = disabledOptions.value.filter(o => o.id !== optionId);
  availableOptions.value.push(option);
  emitUpdate();
};

const moveToDisabled = (optionId) => {
  const option = availableOptions.value.find(o => o.id === optionId);
  availableOptions.value = availableOptions.value.filter(o => o.id !== optionId);
  disabledOptions.value.push(option);
  emitUpdate();
};

const emitUpdate = () => {
  const updatedValue = {};
  [...availableOptions.value, ...disabledOptions.value].forEach((option) => {
      updatedValue[option.id] = disabledOptions.value.includes(option);
  });
  emit('update', { id: props.field.id, value: updatedValue });
};
</script>

<style>
.options-container {
display: flex;
justify-content: space-between;
gap: 40px;
margin-bottom: 30px;
width: max-content;
}

.available-options, .disabled-options {
flex: 1;
background-color: #333;
border-radius: 8px;
padding: 15px;
color: white;
margin: 0;
width: 200px;
height: 40vh;
}

h3 {
font-size: 18px;
margin-bottom: 15px;
}

.option {
padding: 10px;
margin: 5px 0;
background-color: #444;
border-radius: 5px;
cursor: pointer;
transition: background-color 0.3s ease;
}

.option:hover {
background-color: #555;
}

.option.disabled {
background-color: #666;
text-decoration: line-through;
cursor: not-allowed;
}
</style>