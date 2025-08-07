<template>
  <div :class="['input-wrapper', { 'has-error': errors?.length > 0 }]">
    <div v-if="icon" class="input-icon">
      <img :src="icon" alt="icon" />
    </div>
    <input
      v-bind="$attrs"
      class="input"
      :placeholder="placeholder"
      :autocomplete="autocomplete"
      :type="type"
      :style="{ paddingLeft: icon ? '40px' : undefined }"
    />
  </div>
</template>

<script setup lang="ts">
interface Props {
  placeholder?: string
  autocomplete?: string
  type?: string
  icon?: string
  errors?: string[]
}

withDefaults(defineProps<Props>(), {
  errors: () => [],
})
</script>

<style scoped>
.input-wrapper {
  position: relative;
  width: 100%;
}

.input {
  width: 100%;
  padding: 12px 16px;
  font-size: 14px;
  border-radius: 12px;
  background-color: #f5f5f5;
  border: 1px solid #f5f5f5;
  color: #333;
  box-sizing: border-box;
  transition: border-color 0.2s ease;
}

.input:focus {
  outline: none;
  border: 1px solid #107fd1;
  background-color: #fff;
}

.has-error .input {
  border-color: #ff3b30;
}

.input-icon {
  position: absolute;
  top: 50%;
  left: 12px;
  transform: translateY(-50%);
  width: 24px;
  height: 24px;
}

.input-icon img {
  width: 100%;
  height: 100%;
}

.input::placeholder {
  color: #929299;
  font-size: 14px;
}
</style>
