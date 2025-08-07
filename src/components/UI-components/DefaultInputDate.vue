<template>
  <div
    class="datetime-section date-section"
    :class="[{ 'has-error': errors?.length > 0 }]"
    @click="openDatePicker"
  >
    <img :src="CalendarIcon" alt="" class="input-icon" />
    <div class="text-group">
      <div class="label">Дата</div>
      <div class="value">{{ formattedDate || '—' }}</div>
    </div>
    <input ref="dateInput" type="date" v-model="dateValue" autocomplete="off" class="input-date" />
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import CalendarIcon from '@/assets/icons/calendar.svg'

interface Props {
  errors?: string[]
}

withDefaults(defineProps<Props>(), {
  errors: () => [],
})

const dateValue = ref('')

const formattedDate = computed(() => {
  if (!dateValue.value) return ''
  const [year, month, day] = dateValue.value.split('-')
  return `${day}.${month}.${year}`
})

const dateInput = ref<HTMLInputElement | null>(null)

function openDatePicker() {
  dateInput.value?.showPicker?.() || dateInput.value?.focus()
}
</script>

<style scoped>
.datetime-section {
  position: relative;
  background: #f0f0f0;
  border-radius: 12px;
  border: 1px solid #f0f0f0;
  padding: 6px 21px 5px 12px;
  cursor: pointer;
  user-select: none;
  display: flex;
  align-items: center;
}

.date-section {
  flex: 1 1 140px;
  gap: 12px;
}

.time-section {
  padding: 13px 30px 13px 12px;
  justify-content: center;
  font-weight: 600;
  font-size: 14px;
  color: #444;
}

.input-icon {
  width: 24px;
  height: 24px;
  flex-shrink: 0;
}

.text-group {
  display: flex;
  flex-direction: column;
  justify-content: center;
  color: #666;
  font-size: 12px;
  user-select: none;
}

.label {
  font-weight: 400;
  color: #adaeb2;
}

.value {
  font-weight: 500;
  font-size: 14px;
  color: #222;
}

.time-text .value {
  font-weight: 500;
  font-size: 14px;
  color: #222;
}

.input-date,
.input-time {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  cursor: pointer;
  left: 0;
  top: 0;
  border: none;
  background: transparent;
}

.has-error {
  border-color: #ff3b30;
}
</style>
