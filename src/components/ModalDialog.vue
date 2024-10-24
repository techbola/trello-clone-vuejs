<template>
  <div
    v-if="isOpen"
    class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50"
    role="dialog"
    aria-modal="true"
    @click.self="emit('close-modal')"
    @keydown.esc="emit('close-modal')"
    ref="modalEl"
  >
    <div class="w-full max-w-md p-5 bg-white rounded">
      <h2 class="mb-4 text-xl font-bold">
        {{ mode === 'add' ? 'Add New Card' : 'Edit Card' }}
      </h2>
      <input
        v-model="localCard.title"
        type="text"
        placeholder="Card Title"
        aria-label="Card Title"
        class="w-full p-2 mb-4 border rounded"
        ref="titleInputRef"
      />
      <textarea
        v-model="localCard.description"
        class="w-full p-2 mb-4 border rounded"
        placeholder="Description"
        aria-label="Card Description"
      ></textarea>
      <div class="flex justify-end gap-2">
        <button
          class="px-4 py-2 text-black bg-gray-300 rounded hover:bg-gray-200"
          @click="emit('close-modal')"
        >
          Cancel
        </button>
        <button
          class="px-4 py-2 text-white bg-blue-500 rounded hover:bg-blue-600"
          @click="emit('save-modal', localCard)"
          :disabled="localCard.title === ''"
        >
          {{ mode === 'add' ? 'Add' : 'Save' }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { nextTick, ref, watch } from 'vue'
import { useFocusTrap } from '@vueuse/integrations/useFocusTrap'
import type { Card } from '@/types'

const props = defineProps<{
  isOpen: boolean
  card: Card | null
  mode: 'add' | 'edit'
}>()

const emit = defineEmits<{
  (e: 'close-modal'): void
  (e: 'save-modal', card: Card): void
}>()

const titleInputRef = ref<HTMLInputElement | null>(null)
const modalEl = ref<HTMLElement | null>(null)
const localCard = ref<Card>({
  id: 0,
  title: '',
  description: '',
})

const { activate, deactivate } = useFocusTrap(modalEl)

watch(
  () => props.card,
  newCard => {
    if (newCard) {
      localCard.value = { ...newCard }
    } else {
      localCard.value = {
        id: 0,
        title: '',
        description: '',
      }
    }
  },
  {
    immediate: true,
  },
)

watch(
  () => props.isOpen,
  async isOpen => {
    if (isOpen) {
      await nextTick()
      activate()
      titleInputRef.value?.focus()
    } else {
      deactivate()
    }
  },
)
</script>

<style scoped></style>
