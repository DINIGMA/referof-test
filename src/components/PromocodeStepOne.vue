<template>
  <Form keep-values :validation-schema="schema" @submit="onSubmit">
    <div class="promo-modal__block-form">
      <label class="promo-modal__subtitle" for="name">Название промокода<span>*</span></label>
      <Field
        name="name"
        v-slot="{ field, errors }"
        :validate-on-blur="false"
        :validate-on-change="false"
        :validate-on-input="false"
      >
        <DefaultInput
          v-bind="field"
          :placeholder="'Введи название'"
          :autocomplete="'off'"
          :errors="errors"
          type="text"
          id="name"
          @input="field.onInput($event.target.value.toUpperCase())"
        ></DefaultInput>
      </Field>
      <ErrorMessage name="name" class="promo-modal__error-message"></ErrorMessage>
    </div>

    <div class="promo-modal__block-form">
      <label class="promo-modal__subtitle" for="title">Заголовок<span>*</span></label>
      <Field
        name="title"
        v-slot="{ field, errors }"
        :validate-on-blur="false"
        :validate-on-change="false"
        :validate-on-input="false"
      >
        <DefaultInput
          v-bind="field"
          :placeholder="'Введи заголовок'"
          type="text"
          :autocomplete="'off'"
          id="title"
          :errors="errors"
        ></DefaultInput>
      </Field>
      <ErrorMessage name="title" class="promo-modal__error-message"></ErrorMessage>
    </div>

    <div class="promo-modal__block-form">
      <label class="promo-modal__subtitle" for="desc">Сопроводительный текст</label>
      <Field
        name="desc"
        v-slot="{ field, errors }"
        :validate-on-blur="false"
        :validate-on-change="false"
        :validate-on-input="false"
        :validate-on-model-update="false"
      >
        <DefaultTextArea
          v-bind="field"
          label="Сопроводительный текст"
          placeholder="Например: «Ты попал в число счастливчиков! Дарим 300 баллов»"
          :maxlength="250"
          :errors="errors"
          id="desc"
        />
      </Field>
    </div>

    <div class="promo-modal__block-form promo-modal__block-form--last">
      <label class="promo-modal__subtitle" for="points"
        >Укажи количество баллов<span>*</span></label
      >
      <Field
        name="points"
        v-slot="{ field, errors }"
        :validate-on-blur="false"
        :validate-on-change="false"
        :validate-on-input="false"
      >
        <DefaultInput
          id="points"
          v-bind="field"
          :placeholder="'100'"
          :autocomplete="'off'"
          :icon="refIcon"
          :errors="errors"
        ></DefaultInput>
      </Field>
      <ErrorMessage name="points" class="promo-modal__error-message"></ErrorMessage>
    </div>

    <div class="promo-modal__buttons">
      <DefaultButton :text="'Назад'" :variant="'secondary'" type="button"></DefaultButton>
      <DefaultButton :text="'Далее'"></DefaultButton>
    </div>
  </Form>
</template>

<script setup lang="ts">
import DefaultButton from './UI-components/DefaultButton.vue'
import DefaultInput from './UI-components/DefaultInput.vue'
import refIcon from '@/assets/icons/refCoin.svg'
import DefaultTextArea from './UI-components/DefaultTextArea.vue'

import { Form, Field, ErrorMessage } from 'vee-validate'
import { object, string, number } from 'yup'

const emits = defineEmits<{
  (e: 'next-step', values: any): void
  (e: 'prev-step'): void
}>()

// схема валидации
const schema = object({
  name: string().required('Поле обязательно!').max(30, 'Максимальное число символов - 30!'),
  title: string().required('Поле обязательно!').max(120, 'Максимальное число символов - 120!'),
  desc: string().max(250),
  points: number().required('Поле обязательно!').typeError('Введите число'),
})

function onSubmit(values: any) {
  const { desc, ...rest } = values

  if (desc) {
    emits('next-step', values)
  } else {
    emits('next-step', { ...rest })
  }
}
</script>

<style scoped>
.promo-modal__block-form {
  margin-bottom: 1rem;
}

.promo-modal__subtitle {
  display: block;
  margin-bottom: 12px;
  color: #1c1c1e;
}

.promo-modal__subtitle span {
  color: #ff3b30;
}

.promo-modal__block-form--last {
  margin-bottom: 20px;
}

.promo-modal__buttons {
  display: flex;
  justify-content: space-between;
  gap: 12px;
}

.promo-modal__error-message {
  display: block;
  margin-top: 4px;
  color: #ff3b30;
}
</style>
