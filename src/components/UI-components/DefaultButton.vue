<template>
  <button :class="['button', variantClass]">{{ text }}</button>
</template>

<script setup lang="ts">
const props = defineProps<{
  text: string
  variant?: 'accent' | 'secondary'
}>()

const variantClass = props.variant ?? 'accent'
</script>

<style scoped>
.button {
  width: 100%;
  border: none;
  border-radius: 12px;
  padding: 12px 16px;

  text-align: center;
  font-size: 14px;
  font-weight: 600;

  cursor: pointer;
  transition: 0.2s;
}

.button:hover {
  transform: translateY(-2px);
}

.button:active {
  transform: translateY(0px);
  outline: none;
}

.accent {
  background-color: #05263f; /* или твой цвет */
  color: #fff;
}

.accent:hover {
  background-color: #0f4a77; /* или твой цвет */
  color: #fff;
}

.secondary {
  background-color: #f2f1f6;
  color: #77777c;
}
.secondary:hover {
  background-color: #d2d1d3;
  color: #ffffff;
}
</style>
