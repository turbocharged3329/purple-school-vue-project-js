<template>
  <main class="w-main-container">
    <section class="w-main-stats">
      <WStat v-for="stat in displayingStats" :key="stat.id" v-bind="stat" />
    </section>

    <template v-if="weatherData">
      <section class="w-main-days">
        <template v-for="(day,i) in weatherData.forecast.forecastday" :key="day.date">
          <WDayCard :icon-src="day.day.condition.icon" :date="day.date" :temperature="day.day.avgtemp_c" :is-active="activeDayIndex === i" @click="onDayCardClick(i)"/>
        </template>
      </section>
    </template>

    <WCityInput @save-city="onSaveCity" :error-message="errorMessage" />
  </main>
</template>

<script setup>
import { ref, computed } from 'vue';
import WStat from "./components/WStat.vue";
import WCityInput from "./components/WCityInput.vue";
import WDayCard from "./components/WDayCard.vue";

const API_URL = 'http://api.weatherapi.com/v1'

const weatherData = ref(null);
const humidity = ref(0);
const cloud = ref(0);
const wind = ref(0);
const errorMessage = ref('');
const activeDayIndex = ref(null);

const displayingStats = computed(() => [
  {
    id: 1,
    label: "Влажность",
    units: "%",
    value: humidity.value
  },
  {
    id: 2,
    label: "Облачность",
    units: "%",
    value: cloud.value
  },
  {
    id: 3,
    label: "Ветер",
    units: "м/с",
    value: wind.value
  },
])

async function onSaveCity(cityName) {
  const params = new URLSearchParams({
    key: import.meta.env.VITE_API_KEY,
    q: cityName,
    days: 4,
    lang: 'ru'
  })

  try {
    const response = await fetch(`${API_URL}/forecast.json?${params.toString()}`);
    const data = await response.json();

    if (response.status !== 200) {
      console.log("Ошибка при получении данных");

      if (response.status === 400) {
        errorMessage.value = "Город не найден";
      }

      return;
    }

    errorMessage.value = '';

    weatherData.value = data;
    humidity.value = data.current.humidity;
    cloud.value = data.current.cloud;
    wind.value = data.current.wind_kph;
  } catch (error) {
    console.log(error);
  }
} 

function onDayCardClick(index) {
  activeDayIndex.value = index;
}
</script>

<style scoped>
.w-main-container {
  width: 67.5vw;
  background-color: var(--color-bg-main);
  box-sizing: border-box;
  padding: 3.5rem 3.125rem 3.875rem;
  /* 56px 50px 62px */
  border-radius: 1.5625rem;
  /* 25px */
}

.w-main-days {
  display: flex;
  gap: 2px;
  margin-bottom: 4.4375rem;
}

.w-main-stats {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  justify-content: space-between;
  margin-bottom: 5rem;
}
</style>
