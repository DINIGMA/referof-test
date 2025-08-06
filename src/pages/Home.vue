<template>
  <DefaultButton
    :text="'Создать промокод'"
    :variant="'accent'"
    @click="isOpen = true"
  ></DefaultButton>
  <Teleport to="body"
    ><Transition name="fade"
      ><PromocodeModal v-if="isOpen" @close-modal="isOpen = false"></PromocodeModal></Transition
  ></Teleport>
</template>

<script setup lang="ts">
import DefaultButton from '@/components/UI-components/DefaultButton.vue'
import PromocodeModal from '@/components/PromocodeModal.vue'
import { ref, Teleport } from 'vue'

const isOpen = ref<boolean>(false)
</script>

<style></style>
