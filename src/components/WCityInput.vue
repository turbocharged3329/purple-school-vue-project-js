<template>
    <div class="w-city-input">
        <div class="w-city-input-wrapper">
        <input class="w-city-input__input" v-if="isInputVisible" type="text" placeholder="Введите город"
            v-model="cityName" @keyup.enter="toggleInput" />
        <WButton class="w-city-input__button" @click="toggleInput">
            <template #icon>
                <WLocationIcon />
            </template>

            {{ buttonText }}
        </WButton>
    </div>
        <p v-if="errorMessage" class="w-city-input__error">{{ errorMessage }}</p>
    </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import WButton from './WButton.vue';
import WLocationIcon from './WLocationIcon.vue';

const emit = defineEmits(['save-city'])

defineProps({
    errorMessage: {
        type: String,
        default: ''
    }
})

const cityName = ref('Vladimir');
const isInputVisible = ref(false);
const buttonText = computed(() => isInputVisible.value ? 'Сохранить' : 'Изменить город');

onMounted(() => {
    emitCityName(cityName.value);
})

function emitCityName(cityNameValue) {
    emit('save-city', cityNameValue);
}

function toggleInput() {
    if (isInputVisible.value) {
        emitCityName(cityName.value);
    }

    isInputVisible.value = !isInputVisible.value;
}
</script>

<style scoped>
.w-city-input-wrapper {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.w-city-input__input {
    width: 100%;
    height: 53px;
    border: none;
    border-radius: 10px;
    outline: none;
    box-sizing: border-box;
    padding: 1rem;
    background-color: var(--color-bg--dark);
    color: var(--color-primary);
    margin-right: .75rem;
    font-size: 18px;
    flex-shrink: 1;
}

.w-city-input__input::placeholder {
    color: var(--color-text-dark);
}

.w-city-input__button {
    flex-shrink: 2;
}

.w-city-input__error {
    color: var(--color-error);
    font-size: 14px;
    margin-left: 1rem;
    flex: 1;
}
</style>