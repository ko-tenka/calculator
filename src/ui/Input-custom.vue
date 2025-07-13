<template>
  <div class="custom-input">
    <label v-if="label" class="custom-input__label">{{ label }}</label>
    <input
      :type="type"
      :min="min"
      :max="max"
      :step="step"
      :placeholder="placeholder"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      class="custom-input__field"
    />
  </div>
</template>

<script setup>
defineProps({
  modelValue: [String, Number],
  label: String,
  type: {
    type: String,
    default: 'text'
  },
  min: Number,
  max: Number,
  step: Number,
  placeholder: String
});
defineEmits(['update:modelValue']);
</script>

<style scoped>
.custom-input {
  margin-bottom: 14px; 
  } 
.custom-input__label {
  display: block;
  margin-bottom: 6px;
  font-weight: 600;
}
.custom-input__field {
  width: 17vw;
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  transition: border 0.2s;
}
.custom-input__field:focus {
  outline: none;
   box-shadow: 0 0 0 2px rgba(167, 79, 255, 0.232);
 
}
</style>