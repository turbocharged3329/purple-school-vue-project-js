<template>
    <div class="w-day-card" :class="{ 'w-day-card--active': isActive }">
        <img v-if="iconSrc" class="w-day-card__icon" :src="iconSrc" />
        <span v-if="weekday" class="w-day-card__weekday">{{ weekday }}</span>
        <span v-if="temperature" class="w-day-card__temperature">{{ temperature }}</span>
    </div>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
    iconSrc: {
        type: String,
        default: null
    },
    date: {
        type: String,
        default: null
    },
    temperature: {
        type: Number,
        default: null
    },
    isActive: {
        type: Boolean,
        default: false
    }
})

const weekday = computed(() => {
    return new Date(props.date).toLocaleDateString('ru-RU', { weekday: 'short' });
})
const temperature = computed(() => {
    return `${Math.round(props.temperature)}°C`
})
</script>

<style scoped>
.w-day-card {
    min-height: 10.5rem;
    background-color: var(--color-bg--dark);
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: 1.25rem;
    color: var(--color-primary);
    border-radius: 0.625rem;
    cursor: pointer;
}

.w-day-card__icon {
    margin-bottom: 1.125rem;
}

.w-day-card__weekday {
    font-weight: 400;
    margin-bottom: 0.875rem;
    text-transform: uppercase;
}

.w-day-card__temperature {
    font-weight: 600;
}

.w-day-card:not(.w-day-card--active):hover {
    background-color: var(--color-bg--gray);
}

.w-day-card--active {
    background-color: var(--color-primary);
    color: var(--color-secondary);
}
</style>