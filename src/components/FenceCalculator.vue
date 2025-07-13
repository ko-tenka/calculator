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
                  { value: '1.5', text: '1.5' },
                  { value: '1.8', text: '1.8' },
                  { value: '2.0', text: '2.0' }
                ]"
              />

              <InsutCustomSelect
                label="Толщина прутка (мм):"
                v-model="thickness"
                :options="[
                  { value: '1.6', text: '1.6' },
                  { value: '2.2', text: '2.2' },
                  { value: '2.5', text: '2.5' }
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
             min="1"
            />

            <InsutCustomSelect
                label="Откатные ворота (м)"
                v-model="slidingGate"
                :options="[
                  { value: '0', text: 'Нет' },
                  { value: '3', text: '3 м' },
                  { value: '4', text: '4 м' },
                  { value: '5', text: '5 м' },
                  { value: '7', text: '7 м' },
                  { value: '9', text: '9 м' },
                ]"
              />


          </div>
          <div class="fence-calculator__section">

              <InsutCustomSelect
                label="Распашные ворота (м):"
                v-model="swingGate"
                :options="[
                  { value: '0', text: 'Нет' },
                  { value: '3.45', text: '3.45 м' },            
                  { value: '4.35', text: '4.35 м' },              
                  { value: '5.25', text: '5.25 м' },                  
                  { value: '7.15', text: '7.15 м' },                  
                ]"
              />

            <InputCustom
             label="Количество калиток:"
             type="number"
             v-model="gatesCount"
             min="0" 
             max="3"
            />


            <InsutCustomSelect
                label="Автопривод:"
                v-model="autodrive"
                :options="[
                  { value: '0', text: 'Нет' },
                  { value: '20000', text: 'Китай (20 000₽' },            
                  { value: '25000', text: 'Италия (25 000₽)' },              
                  { value: '30000', text: 'Германия (30 000₽)' },                
                ]"
              />

            <InsutCustomSelect
                label="Врезной замок:"
                v-model="lock"
                :options="[
                  { value: '0', text: 'Нет' },
                  { value: '4500', text: 'Есть (4 500₽)' },                          
                ]"
              />



            <InputCustom
             label="Расстояние доставки (км):"
             type="number"
             v-model="deliveryDistance"
            />

          </div>
        </div>
        <button class="fence-calculator__button" @click="showTotal = true">Рассчитать</button>
        <h2 v-if="showTotal" class="fence-calculator__total">Итоговая стоимость: {{ totalFormatted }} ₽</h2>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import Calc from '/calculator-2.png'

import InputCustom from '../ui/Input-custom.vue'
import InsutCustomSelect from '../ui/Input-custom-selection.vue'

// Реактивные данные
const height = ref('1.5')
const thickness = ref('1.6')
const painting = ref('0')
const length = ref(10)
const slidingGate = ref('0')
const swingGate = ref('0')
const gatesCount = ref(1)
const autodrive = ref('0')
const lock = ref('0')
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

const gatePrice = 5500
const manipulatorPrice = 8500
const deliveryPerKm = 80

const swingGatePrices = {
  "3.45": 6500,
  "4.35": 7500,
  "5.25": 8500,
  "7.15": 9500
}

const slidingGatePrices = {
  "3": 20000,
  "4": 25000,
  "5": 30000,
  "7": 35000,
  "9": 40000
}

// Вычисляемая стоимость
const total = computed(() => {
  const key = `${height.value}_${thickness.value}_${painting.value}`
  const perMeter = basePrices[key] || 1000

  let total = perMeter * length.value

  // Добавляем стоимость калиток
  total += gatesCount.value * gatePrice

  // Добавляем стоимость распашных ворот
  total += swingGatePrices[swingGate.value] || 0

  // Добавляем стоимость откатных ворот
  total += slidingGatePrices[slidingGate.value] || 0

  // Добавляем стоимость автопривода
  total += parseInt(autodrive.value)

  // Добавляем стоимость замка
  total += parseInt(lock.value)

  // Добавляем стоимость доставки
  total += manipulatorPrice + (deliveryDistance.value * deliveryPerKm)

  return total
})

// Форматированная стоимость
const totalFormatted = computed(() => {
  return total.value.toLocaleString('ru-RU')
})
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

label {
  display: block;
  margin: 10px 0;
  font-weight: bold;
}

select, input {
  display: block;
  width: 100%;
  padding: 8px;
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
}

select:focus, input:focus {
  outline: none;
  border-color: #007bff;
  box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.25);
}

.fence-calculator__total {
  color: var( --fence-total-bg);
  text-align: center;
  padding: 20px;
  background-color: #d4edda;
  border-radius: 8px;
  border: 1px solid #c3e6cb;
  margin-top: 32px;
}
.fence-calculator__button {
  display: block;
  margin: 32px auto 0 auto;
  padding: 12px 32px;
  font-size: 18px;
  background: #667eea;
  color: #fff;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.2s;
}
.fence-calculator__button:hover {
  background: #4953b8;
}
</style> 