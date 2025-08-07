<template>
  <div class="promo-modal">
    <div class="promo-modal__content">
      <!-- Иконка закрытия формы -->
      <div class="close-icon">
        <CloseIcon @click="$emit('close-modal')"></CloseIcon>
      </div>

      <!-- Контент -->
      <div class="promo-modal__body">
        <h2 class="promo-modal__title">Создание промокода</h2>
        <div class="promo-modal__state">
          <div class="promo-modal__state-step" :class="{ active: step === 1 }">
            <span> Шаг 1: Основное </span>
          </div>
          <div class="promo-modal__state-step" :class="{ active: step === 2 }">
            <span>Шаг 2: Настройки промокода</span>
          </div>
        </div>

        <!-- Форма -->
        <div class="promo-modal__form">
          <Transition name="slide-fade" mode="out-in">
            <KeepAlive>
              <component :is="currentStepComponent" @next-step="nextStep" @prev-step="prevStep" />
            </KeepAlive>
          </Transition>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import CloseIcon from './UI-components/icons/CloseIcon.vue'
import PromocodeStepOne from './PromocodeStepOne.vue'
import PromocodeStepTwo from './PromocodeStepTwo.vue'

const emits = defineEmits<{
  (e: 'close-modal'): void
}>()

const step = ref<number>(1)

const formStepOneData = ref({})
const formStepTwoData = ref({})

const currentStepComponent = computed(() =>
  step.value === 1 ? PromocodeStepOne : PromocodeStepTwo,
)

const nextStep = (data: any) => {
  if (step.value === 1) {
    formStepOneData.value = data
    step.value = 2
  } else if (step.value === 2) {
    formStepTwoData.value = data

    console.log('Данные формы', {
      ...formStepOneData.value,
      ...formStepTwoData.value,
    })

    emits('close-modal')
  }
}

const prevStep = () => {
  step.value = 1
}
</script>

<style scoped>
.promo-modal {
  position: fixed;
  inset: 0;
  background: hsla(0, 0%, 7%, 0.15);
  display: flex;
  align-items: center;
  justify-content: center;
}
.promo-modal__content {
  background: #fff;
  position: relative;
  padding: 24px;
  border-radius: 16px;
  width: 516px;
}

.promo-modal__title {
  margin-bottom: 1rem;
  font-size: 20px;
  font-weight: 600;
}

/* TODO: если будет время анимировать переход!! */
.promo-modal__state {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
  position: relative;
  padding: 0px 12px;
  gap: 10px;
  color: #77777c;
  border-bottom: 1px #e5e5ea solid;
}

.promo-modal__state-step {
  position: relative;
  padding: 8px 6px;
  width: 100%;

  text-align: center;
  font-weight: 500;
  transition: all 0.3s ease;
}

.promo-modal__state-step.active {
  color: #107fd1;
  font-weight: 600;
}

.promo-modal__state-step.active::after {
  content: '';
  position: absolute;

  bottom: -1px;
  left: 0;
  height: 2px;

  width: 100%;
  background-color: #107fd1;
}

.close-icon {
  position: absolute;

  top: 10px;
  right: 10px;
  width: 24px;
  height: 24px;

  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
