<template>
<div class="w-stat">
    <span class="w-stat__label">
        {{ label }}
    </span>
    <span class="w-stat__value">
        {{ valueWithUnits }}
    </span>
</div>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
    value: {
        type: [Number, String],
        default: 0
    },
    label: {
        type: String,
        default: ''
    },
    units: {
        type: String,
        default: ''
    },
})

const valueWithUnits = computed(() => {
    return `${props.value}${props.units}`
})
</script>

<style scoped>
.w-stat {
    display: flex;
    justify-content: space-between;
    font-size: 1.75rem;
}

.w-stat__label {
    font-weight: 700;
    text-transform: uppercase;
}

.w-stat__value {
    font-weight: 500;
}
</style>