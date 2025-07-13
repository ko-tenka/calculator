<!-- Custom Button Component -->
<template #button>
  <button
    :type="buttonType"
    class="custom-btn"
    @click="$emit('click')"
  >
    {{ buttonLabel }}
  </button>
</template>

<script setup>
defineProps({
  buttonLabel: {
    type: String,
    default: 'Кнопка'
  },
  buttonType: {
    type: String,
    default: 'button'
  }
});
defineEmits(['click']);
</script>

<style scoped>


 .custom-btn  {
  /* display: block; */
  margin: 12px auto 0 auto;
  padding: 8px 5vw;
  font-size: 18px;
  background: linear-gradient(90deg, #202020 10%, #9551c0 90%);
  color: #fff;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.2s;
  box-shadow: 0 2px 8px rgba(167, 79, 255, 0.12);
  transition: background 0.2s, box-shadow 0.2s;
  font-weight: 600;
}

.custom-btn:hover {
  background: linear-gradient(90deg, #000000 10%, #8244a8 90%);
  box-shadow: 0 4px 16px rgba(167, 79, 255, 0.18);
}
</style>
