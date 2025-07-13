<template>
  <div class="custom-input-select">
    <label v-if="label" class="custom-input-select__label">{{ label }}</label>
    <select
      :value="modelValue"
      @change="$emit('update:modelValue', $event.target.value)"
      class="custom-input-select__field"
    >
      <option
        v-for="option in options"
        :key="option.value"
        :value="option.value"
      >
        {{ option.text }}
      </option>
    </select>
  </div>
</template>

<script setup>
defineProps({
  modelValue: [String, Number],
  label: String,
  options: {
    type: Array,
    required: true // [{ value: '1', text: 'Один' }, ...]
  }
});
defineEmits(['update:modelValue']);
</script>

<style scoped>
.custom-input-select{
  margin-bottom: 14px; 
}
.custom-input-select__label {
  display: block;
  margin-bottom: 6px;
  font-weight: 600;
}
.custom-input-select__field {
  width: 19vw;
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  transition: border 0.2s;
}
.custom-input-select__field:focus {
  outline: none;
  box-shadow: 0 0 0 2px rgba(167, 79, 255, 0.232);
}
.custom-input-select__field option {
  margin-top: 10px;
  background: #ad1010;
  color: #333;
  font-size: 16px;
}
</style> 