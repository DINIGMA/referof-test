<template>
  <Form
    :validation-schema="schema"
    @submit="onSubmit"
    :initial-values="{ promoType: 'not-send', withoutDateEnd: false }"
  >
    <div class="promo-modal__block-form">
      <label class="promo-modal__subtitle" for="name">Срок действия промокода<span>*</span></label>
      <div class="promo-modal__date-group">
        <div class="promo-modal__datetime-content">
          <h4>Дата начала</h4>
          <div class="promo-modal__datetime">
            <Field
              name="date-start"
              v-slot="{ field, errors }"
              :validate-on-blur="false"
              :validate-on-change="false"
              :validate-on-input="false"
            >
              <DefaultInputDate v-bind="field" :errors="errors"></DefaultInputDate>
            </Field>
            <Field
              name="time-start"
              v-slot="{ field, errors }"
              :validate-on-blur="false"
              :validate-on-change="false"
              :validate-on-input="false"
              ><DefaultInputTime v-bind="field" :errors="errors"></DefaultInputTime>
            </Field>
          </div>
        </div>
        <div class="promo-modal__datetime-content">
          <h4>Дата конца</h4>
          <div class="promo-modal__datetime">
            <Field
              name="date-end"
              v-slot="{ field, errors }"
              :validate-on-blur="false"
              :validate-on-change="false"
              :validate-on-input="false"
            >
              <DefaultInputDate v-bind="field" :errors="errors"></DefaultInputDate>
            </Field>
            <Field
              name="time-end"
              v-slot="{ field, errors }"
              :validate-on-blur="false"
              :validate-on-change="false"
              :validate-on-input="false"
              ><DefaultInputTime v-bind="field" :errors="errors"></DefaultInputTime>
            </Field>
          </div>
        </div>
      </div>

      <div class="promo-modal__checkbox">
        <Field
          name="withoutDateEnd"
          type="checkbox"
          :value="true"
          :unchecked-value="false"
          v-slot="{ field, errors, meta }"
        >
          <label class="checkbox-label">
            <input type="checkbox" v-bind="field" class="checkbox-input" />
            <span class="checkbox-box"></span>
            <span class="checkbox-text">Без даты конца</span>
          </label>
        </Field>
      </div>
      <ErrorMessage name="date-start" class="promo-modal__error-message"></ErrorMessage>
      <ErrorMessage name="date-end" class="promo-modal__error-message"></ErrorMessage>
    </div>

    <div class="line"></div>

    <div class="promo-modal__block-form">
      <label class="promo-modal__subtitle" for="activationLimit"
        >Введите лимит активаций (шт.)<span>*</span></label
      >
      <Field
        name="activationLimit"
        v-slot="{ field, errors }"
        :validate-on-blur="false"
        :validate-on-change="false"
        :validate-on-input="false"
      >
        <DefaultInput
          v-bind="field"
          :placeholder="'1000'"
          type="text"
          :autocomplete="'off'"
          id="activationLimit"
          :errors="errors"
        ></DefaultInput>
      </Field>
      <ErrorMessage name="activationLimit" class="promo-modal__error-message"></ErrorMessage>
    </div>

    <div class="promo-modal__block-form">
      <p class="promo-modal__subtitle promo-modal__subtitle--accent">
        Настроить получение промокода
      </p>

      <div class="promo-modal__radio-group">
        <Field
          name="promoType"
          type="radio"
          v-slot="{ field }"
          :value="'not-send'"
          :validate-on-change="false"
        >
          <label class="custom-radio">
            <input type="radio" value="not-send" v-bind="field" />
            <span class="custom-radio-box"></span>
            <span class="custom-radio-label">Создать промокод без отправки</span>
          </label>
        </Field>
        <Field
          name="promoType"
          type="radio"
          v-slot="{ field }"
          :value="'send-all'"
          :validate-on-change="false"
        >
          <label class="custom-radio">
            <input type="radio" value="send-all" v-bind="field" />
            <span class="custom-radio-box"></span>
            <span class="custom-radio-label">Отправить промокод всем пользователям</span>
          </label>
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
      <DefaultButton :text="'Создать'"></DefaultButton>
    </div>
  </Form>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue'
import DefaultButton from './UI-components/DefaultButton.vue'
import DefaultInput from './UI-components/DefaultInput.vue'
import DefaultInputDate from './UI-components/DefaultInputDate.vue'
import DefaultInputTime from './UI-components/DefaultInputTime.vue'

import { Form, Field, ErrorMessage } from 'vee-validate'

import { object, string, number, boolean } from 'yup'

const emits = defineEmits<{
  (e: 'next-step', values: any): void
  (e: 'prev-step'): void
}>()

const schema = object({
  'date-start': string().required('Дата начала обязательна'),
  'time-start': string().required('Время начала обязательно'),

  withoutDateEnd: boolean().default(false),

  'date-end': string().when('withoutDateEnd', {
    is: false,
    then: (schema) => schema.required('Дата конца обязательна'),
    otherwise: (schema) => schema.optional(),
  }),

  'time-end': string().when('withoutDateEnd', {
    is: false,
    then: (schema) => schema.required('Время конца обязательно'),
    otherwise: (schema) => schema.optional(),
  }),

  activationLimit: number().typeError('Введите число').required('Лимит активаций обязателен'),

  promoType: string().required('Выберите способ получения промокода'),
})
  .test(
    'start-date-after-now',
    'Дата и время начала должны быть позже текущего времени',
    function (values) {
      const { 'date-start': dateStart, 'time-start': timeStart } = values
      if (!dateStart || !timeStart) return true

      const start = new Date(`${dateStart}T${timeStart}`)

      const now = new Date()

      if (start <= now) {
        return this.createError({
          path: 'date-start',
          message: 'Дата и время начала должны быть позже текущего времени',
        })
      }

      return true
    },
  )
  .test(
    'end-date-after-start',
    'Дата и время конца должны быть позже даты и времени начала',
    function (values) {
      const {
        'date-start': dateStart,
        'time-start': timeStart,
        'date-end': dateEnd,
        'time-end': timeEnd,
        withoutDateEnd,
      } = values

      if (withoutDateEnd || !dateEnd || !timeEnd) return true

      const start = new Date(`${dateStart}T${timeStart}`)
      const end = new Date(`${dateEnd}T${timeEnd}`)

      if (end <= start) {
        return this.createError({
          path: 'date-end',
          message: 'Дата и время конца должны быть позже даты и времени начала',
        })
      }

      return true
    },
  )

function onSubmit(values: any) {
  const {
    'date-start': dateStart,
    'time-start': timeStart,
    'date-end': dateEnd,
    'time-end': timeEnd,
    withoutDateEnd,
    ...rest
  } = values

  const result: Record<string, any> = {
    ...rest,
    dateStart: new Date(`${dateStart}T${timeStart}`),
  }

  if (!withoutDateEnd && dateEnd && timeEnd) {
    result.dateEnd = new Date(`${dateEnd}T${timeEnd}`)
  }

  emits('next-step', result)
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

.promo-modal__subtitle--accent {
  font-weight: 600;
  color: #05263f;
}
.promo-modal__subtitle span {
  color: #ff3b30;
}

.promo-modal__block-form--last {
  margin-bottom: 20px;
}

.promo-modal__datetime-content {
  width: 100%;
}

.promo-modal__datetime-content h4 {
  margin-bottom: 6px;

  font-size: 14px;
  font-weight: 500;
  color: #797979;
}

.promo-modal__date-group {
  display: flex;
  gap: 10px;
  margin-bottom: 8px;
}

.promo-modal__datetime {
  display: flex;
  justify-content: space-between;
  gap: 10px;
}

.promo-modal__error-message {
  display: block;
  margin-top: 4px;
  color: #ff3b30;
}

.promo-modal__buttons {
  display: flex;
  justify-content: space-between;
  gap: 12px;
}

.promo-modal__radio-group {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.line {
  border-bottom: 1px solid #ebebf0;
  margin-bottom: 20px;
}

/* Вынес чекбокс и радио в main.css для того чтобы не захламлять компонент */
</style>
