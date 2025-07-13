<template>
  <div class="fence-calculator">
    <h1 class="fence-calculator__title">Калькулятор забора из сетки Рабица</h1>
    <div class="fence-calculator_conteiner">
      <img :src="Calc" alt="Калькулятор" class="fence-calculator__image">
      <div class="fence-calculator__form">
        <div class="fence-calculator__columns">
          <div class="fence-calculator__section">

              <InsutCustomSelect
                label="Высота сетки (м):"
                v-model="height"
                :options="[
                  { value: '1.5', text: '1.5 м' },
                  { value: '1.8', text: '1.8 м' },
                  { value: '2.0', text: '2.0 м' }
                ]"
                
              />

              <InsutCustomSelect
                label="Толщина сетки (мм):"
                v-model="thickness"
                :options="[
                  { value: '1.6', text: '1.6 мм' },
                  { value: '2.2', text: '2.2 мм' },
                  { value: '2.5', text: '2.5 мм' }
                ]"
      
              />

              <InsutCustomSelect
                label="Покраска:"
                v-model="painting"
                :options="[
                  { value: '0', text: 'Без покраски' },
                  { value: '1', text: 'С покраской' }
                ]"

              />

            <InputCustom
             label="Длина забора (м):"
             type="number"
             v-model="length"
             :min="1"
  
            />

            <InsutCustomSelect
                label="Откатные ворота (м)✅"
                v-model="slidingGate"
                :options="[
                  { value: '0', text: 'Нет' },
                  { value: '20000', text: '3 м' },
                  { value: '25000', text: '4 м' },
                  { value: '30000', text: '5 м' },
                  { value: '35000', text: '7 м' },
                  { value: '40000', text: '9 м' },
                ]"
              />


          </div>
          <div class="fence-calculator__section">

              <InsutCustomSelect
                label="Распашные ворота (м):✅"
                v-model="swingGate"
                :options="[
                  { value: '0', text: 'Нет' },
                  { value: '6500', text: '3.45 м' },            
                  { value: '7500', text: '4.35 м' },              
                  { value: '8500', text: '5.25 м' },                  
                  { value: '9500', text: '7.15 м' },                  
                ]"
              />

            <InputCustom
             label="Количество калиток:✅"
             type="number"
             v-model="gatesCount"
             :min="0" 
             :max="3"
        
            />


            <InsutCustomSelect
                label="Автопривод:✅"
                v-model="autodrive"
                :options="[
                  { value: '0', text: 'Нет' },
                  { value: '20000', text: 'Китай (20 000₽)' },            
                  { value: '25000', text: 'Италия (25 000₽)' },              
                  { value: '30000', text: 'Германия (30 000₽)' },                
                ]"
 
              />


            <InputCustom
             label="Врезной замок:✅"
             type="number"
             v-model="lock"
             :min="0" 
             :max="3"
        
            />



            <InputCustom
             label="Расстояние доставки (км):✅"
             type="number"
             v-model="deliveryDistance"
            />

          </div>
        </div>
        <div class="fence-calculator__actions">
          <ButtonCustom @click="showTotal = true">Рассчитать</ButtonCustom>
          <h2 class="fence-calculator__total">Итоговая стоимость: {{ totalFormatted }} ₽</h2>
          <h3 class="fence-calculator__delivery">Стоимость доставки: {{ deliveryCost.toLocaleString('ru-RU') }} ₽</h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import Calc from '/calculator-2.png'

import InputCustom from '../ui/Input-custom.vue'
import InsutCustomSelect from '../ui/Input-custom-selection.vue'
import ButtonCustom from '@/ui/Button-custom.vue'

// Реактивные данные
const height = ref('1.5')
const thickness = ref('1.6')
const painting = ref('0')
const length = ref(10)
const slidingGate = ref('0')
const swingGate = ref('0')
const gatesCount = ref(1)
const autodrive = ref('0')
const lock = ref(0)
const deliveryDistance = ref(10)
const showTotal = ref(false)

// Базовые цены
const basePrices = {
  '1.5_1.6_0': 700,
  '1.5_1.6_1': 750,
  '1.5_2.2_0': 1000,
  '1.8_2.2_0': 1150,
  '2.0_2.5_0': 1400,
  '2.0_2.5_1': 1420,
}

const gatePrice = 4500
const manipulatorPrice = 8500
const deliveryPerKm = 80



// Вычисляемая стоимость
const deliveryCost = computed(() => {
  return manipulatorPrice + (deliveryDistance.value * deliveryPerKm)
})

const total = computed(() => {
  const key = `${height.value}_${thickness.value}_${painting.value}`
  const perMeter = basePrices[key] || 1000

  let total = perMeter * length.value
  total += gatesCount.value * gatePrice
  total += parseInt(swingGate.value) || 0
  total += parseInt(slidingGate.value) || 0
  total += parseInt(autodrive.value)
  total += lock.value * gatePrice
  // Убрали доставку из total
  return total
})

// Форматированная стоимость
const totalFormatted = computed(() => {
  return total.value.toLocaleString('ru-RU')
})

function hideTotal() {
  showTotal.value = false
}
</script>

<style scoped>
.fence-calculator {
  font-family: Arial, sans-serif;
  margin: 20px;
  max-width: 1280px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.fence-calculator__title {
  text-align: center;
  margin-bottom: 20px;
}

.fence-calculator_conteiner{
    display: flex;
}
.fence-calculator__image {
  display: block;
  margin: 5vw 4vw auto 0vw;
  width: 35vw;
  height: 35vw;
}

.fence-calculator__form {
  width: 100%;
}

.fence-calculator__columns {
  display: flex;
  gap: 1vw;
  justify-content: center;
  background-color: var(--fence-columns-bg);
  border-radius: 12px;
}

.fence-calculator__section {
  flex: 1 1 0;
  min-width: 250px;
  margin-bottom: 0;
  padding: 20px;
}

.fence-calculator__actions {
  text-align: right;
  margin-top: 24px;
}
.fence-calculator__total {
  margin-top: 18px;
}
.fence-calculator__delivery {
  margin-top: 10px;
  color: #3a3a3a;
  font-size: 16px;
}
</style> 