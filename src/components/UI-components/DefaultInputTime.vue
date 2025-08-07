<template>
  <div class="datetime-section time-section" @click="openTimePicker">
    <div class="text-group time-text">
      <div class="value">{{ timeValue || '—' }}</div>
    </div>
    <input ref="timeInput" type="time" v-model="timeValue" autocomplete="off" class="input-time" />
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

const timeValue = ref('')

const timeInput = ref<HTMLInputElement | null>(null)

function openTimePicker() {
  timeInput.value?.showPicker?.() || timeInput.value?.focus()
}
</script>

<style scoped>
.datetime-section {
  position: relative;
  background: #f0f0f0;
  border-radius: 12px;
  padding: 6px 21px 5px 12px;
  cursor: pointer;
  user-select: none;
  display: flex;
  align-items: center;
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
</style>
