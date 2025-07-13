<template>
  <div class="w-main-left">
    <section class="w-main-stats">
      <WStat v-for="stat in displayingStats" :key="stat.id" v-bind="stat" />
    </section>

    <template v-if="weatherData">
      <section class="w-main-days">
        <template
            v-for="(day, i) in weatherData.forecast.forecastday"
            :key="day.date"
        >
          <WDayCard
              :icon-src="day.day.condition.icon"
              :date="day.date" :temperature="day.day.avgtemp_c"
              :is-active="activeDayIndex === i" @click="onDayCardClick(i)"
          />
        </template>
      </section>
    </template>

    <WCityInput @save-city="onSaveCity" :error-message="errorMessage" />
  </div>
</template>

<script setup>
import WStat from "./WStat.vue";
import WDayCard from "./WDayCard.vue";
import WCityInput from "./WCityInput.vue";
import {computed, ref} from "vue";

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

function onDayCardClick(index) {
  activeDayIndex.value = index;
}


const API_URL = 'http://api.weatherapi.com/v1'


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
</script>


<style scoped>
.w-main-left {
  width: 36.18vw;
  height: 43.3vw;
  background-color: var(--color-bg-main);
  padding: 3.5rem 3.125rem 3.875rem;
  border-radius: 0 1.5625rem 1.5625rem 0;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

.w-main-stats {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  justify-content: space-between;
  margin-bottom: 5rem;
}

.w-main-days {
  display: flex;
  gap: 2px;
  margin-bottom: auto;
}
</style>