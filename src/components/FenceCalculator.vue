<template>
  <div class="fence-calculator">
    <h1 class="fence-calculator__title">Калькулятор забора из сетки Рабица</h1>
    <div class="fence-calculator_conteiner">
      <img :src="Calc" alt="Калькулятор" class="fence-calculator__image">
      <div class="fence-calculator__form">
        <div class="fence-calculator__columns">
          <div class="fence-calculator__section">
            <label>
              Высота сетки (м):
              <select v-model="height">
                <option value="1.5">1.5</option>
                <option value="1.8">1.8</option>
                <option value="2.0">2.0</option>
              </select>
            </label>
            <label>
              Толщина прутка (мм):
              <select v-model="thickness">
                <option value="1.6">1.6</option>
                <option value="2.2">2.2</option>
                <option value="2.5">2.5</option>
              </select>
            </label>
            <label>
              Покраска:
              <select v-model="painting">
                <option value="0">Без покраски</option>
                <option value="1">С покраской</option>
              </select>
            </label>
            <label>
              Длина забора (м):
              <input type="number" v-model="length" min="1" />
            </label>
          </div>
          <div class="fence-calculator__section">
            <label>
              Откатные ворота (м):
              <select v-model="slidingGate">
                <option value="0">Нет</option>
                <option value="3">3 м</option>
                <option value="4">4 м</option>
                <option value="5">5 м</option>
                <option value="7">7 м</option>
                <option value="9">9 м</option>
              </select>
            </label>
            <label>
              Распашные ворота (м):
              <select v-model="swingGate">
                <option value="0">Нет</option>
                <option value="3.45">3.45 м</option>
                <option value="4.35">4.35 м</option>
                <option value="5.25">5.25 м</option>
                <option value="7.15">7.15 м</option>
              </select>
            </label>
            <label>
              Количество калиток:
              <input type="number" v-model="gatesCount" min="0" max="3" />
            </label>
            <label>
              Автопривод:
              <select v-model="autodrive">
                <option value="0">Нет</option>
                <option value="20000">Китай (20 000₽)</option>
                <option value="25000">Италия (25 000₽)</option>
                <option value="30000">Германия (30 000₽)</option>
              </select>
            </label>
            <label>
              Врезной замок:
              <select v-model="lock">
                <option value="0">Нет</option>
                <option value="4500">Есть (4 500₽)</option>
              </select>
            </label>
            <label>
              Расстояние доставки (км):
              <input type="number" v-model="deliveryDistance" />
            </label>
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
  color: #333;
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
  gap: 32px;
  justify-content: center;
  background-color: #d1cdf688;
  border-radius: 8px;
  border: 1px solid #ddd;
}

.fence-calculator__section {
  flex: 1 1 0;
  min-width: 250px;
  margin-bottom: 0;
  padding: 20px;
  /* border: 1px solid #ddd;
  border-radius: 8px; */
  /* background-color: none; */
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
  color: #28a745;
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