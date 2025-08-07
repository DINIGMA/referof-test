<template>
  <div class="textarea-wrapper" :class="[{ 'has-error': errors?.length > 0 }]">
    <textarea
      v-bind="$attrs"
      class="textarea"
      :placeholder="placeholder"
      v-model="currentValue"
      :id="id"
    ></textarea>
    <div class="char-counter">{{ currentValue.length }}/{{ maxlength }}</div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'

interface Props {
  label?: string
  placeholder?: string
  maxlength?: number
  modelValue?: string
  id?: string
  errors?: string[]
}

const props = withDefaults(defineProps<Props>(), {
  errors: () => [],
})

const currentValue = ref(props.modelValue ?? '')
</script>

<style scoped>
.textarea-wrapper {
  position: relative;
}

.textarea-label {
  display: inline-block;
  font-weight: 500;
  font-size: 14px;
}

.textarea {
  display: block;
  width: 100%;
  min-height: 80px;
  padding: 16px 12px;
  border-radius: 12px;
  background-color: #f5f5f5;
  border: 1px solid #f5f5f5;
  font-size: 14px;
  font-family: Inter, sans-serif;
  color: #333;
  resize: none;
  transition: border-color 0.2s ease;
}

.textarea:focus {
  outline: none;
  border: 1px solid #107fd1;
  background-color: #fff;
}

.textarea::placeholder {
  color: #929299;
  font-size: 14px;
}

.char-counter {
  text-align: right;
  font-size: 12px;
  font-weight: 400;
  color: #adaeb2;
  margin-top: 4px;
}

.has-error textarea {
  border: 1px solid #ff3b30;
}
.has-error .char-counter {
  color: #ff3b30;
}

.has-error:focus {
  border: 1px solid #ff3b30;
}
</style>
