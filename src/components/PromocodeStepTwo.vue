<template>
  <Form :validation-schema="schema" @submit="onSubmit">
    <div class="promo-modal__block-form">
      <label class="promo-modal__input-label" for="name"
        >Срок действия промокода<span>*</span></label
      >
      <div class="my">
        <Field name="date-start" v-slot="{ field, errors, meta }">
          <DefaultInputDate v-bind="field"></DefaultInputDate>
        </Field>
        <Field name="time-start" v-slot="{ field, errors, meta }"
          ><DefaultInputTime v-bind="field"></DefaultInputTime>
        </Field>
      </div>
      <div class="my">
        <Field name="date-end" v-slot="{ field, errors, meta }">
          <DefaultInputDate v-bind="field"></DefaultInputDate>
        </Field>
        <Field name="time-end" v-slot="{ field, errors, meta }"
          ><DefaultInputTime v-bind="field"></DefaultInputTime>
        </Field>
      </div>
    </div>

    <div class="promo-modal__buttons">
      <DefaultButton
        :text="'Назад'"
        :variant="'secondary'"
        type="button"
        @click="$emit('prev-step')"
      ></DefaultButton>
      <DefaultButton :text="'Далее'"></DefaultButton>
    </div>
  </Form>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue'
import DefaultButton from './UI-components/DefaultButton.vue'
import DefaultInput from './UI-components/DefaultInput.vue'
import refIcon from '@/assets/icons/refCoin.svg'
import DefaultInputDate from './UI-components/DefaultInputDate.vue'
import DefaultInputTime from './UI-components/DefaultInputTime.vue'

import { Form, Field, ErrorMessage } from 'vee-validate'
import { object, string, number } from 'yup'

const emits = defineEmits<{
  (e: 'next-step', values: any): void
  (e: 'prev-step'): void
}>()

const schema = object({
  name: string().required().max(30),
  title: string().required().max(120),
  desc: string().max(250),
  points: number().required().typeError('Введите число'),
})

function onSubmit(values: any) {
  emits('next-step', values)
}
</script>

<style scoped>
.promo-modal__block-form {
  margin-bottom: 1rem;
}

.promo-modal__input-label {
  display: block;
  margin-bottom: 6px;
  color: #1c1c1e;
}

.promo-modal__block-form--last {
  margin-bottom: 20px;
}

.promo-modal__input-label span {
  color: #ff3b30;
}

.promo-modal__buttons {
  display: flex;
  justify-content: space-between;
  gap: 12px;
}

.my {
  display: flex;
  justify-content: space-between;
  gap: 10px;
}
</style>
