<template>
  <main class="p-5 font-sans">
    <div class="flex gap-5 py-5 overflow-x-auto">
      <div
        class="bg-gray-100 p-3 rounded-lg min-w-[250px] flex flex-col"
        v-for="(list, listIndex) in lists"
        :key="list.id"
      >
        <h2 class="mb-2 font-medium">{{ list.title }}</h2>

        <Draggable :list="list.cards" group="cards" item-key="id">
          <template #item="{ element }">
            <div
              @click="openModal(listIndex, element)"
              class="p-2 my-2 bg-white rounded shadow cursor-pointer"
            >
              <span class="text-sm font-medium">{{ element.title }}</span>
              <p class="text-xs text-gray-400">{{ element.description }}</p>
            </div>
          </template>
        </Draggable>

        <button
          class="w-full p-2 mt-2 text-sm font-medium text-left text-gray-500 bg-transparent rounded hover:bg-white"
          @click="openModal(listIndex)"
        >
          + Add card
        </button>
      </div>
    </div>

    <ModalDialog
      :is-open="isModalOpen"
      @close-modal="closeModal"
      @save-modal="saveCard"
      :card="editingCard"
      :mode="modalMode"
    />
  </main>
</template>

<script setup lang="ts">
import { computed, reactive, ref } from 'vue'
import Draggable from 'vuedraggable'
import ModalDialog from './components/ModalDialog.vue'
import type { List, Card } from './types'

const lists = reactive<List[]>([
  {
    id: 1,
    title: 'To Do',
    cards: [
      { id: 1, title: 'Task 1', description: 'Description for Task 1' },
      { id: 2, title: 'Task 2', description: 'Description for Task 2' },
    ],
  },
  {
    id: 2,
    title: 'In Progress',
    cards: [
      { id: 3, title: 'Task 3', description: 'Description for Task 3' },
      { id: 4, title: 'Task 4', description: 'Description for Task 4' },
    ],
  },
  {
    id: 3,
    title: 'Done',
    cards: [{ id: 5, title: 'Task 5', description: 'Description for Task 5' }],
  },
])

const isModalOpen = ref(false)
const editingCard = ref<Card | null>(null)
const editingListIndex = ref<number | null>(null)

const modalMode = computed(() => (editingCard.value === null ? 'add' : 'edit'))

const openModal = (listIndex: number, card?: Card) => {
  editingListIndex.value = listIndex
  editingCard.value = card === undefined ? null : card
  isModalOpen.value = true
}

const saveCard = (card: Card) => {
  if (editingListIndex.value === null) return

  const editingList = lists[editingListIndex.value]

  if (modalMode.value === 'add') {
    const newId = Math.max(...editingList.cards.map(c => c.id)) + 1
    editingList.cards.push({
      ...card,
      id: newId,
    })
  } else {
    const cardIndex = editingList.cards.findIndex(c => c.id === card.id)

    if (cardIndex === -1) return

    editingList.cards[cardIndex] = card
  }
  closeModal()
}

const closeModal = () => {
  isModalOpen.value = false
  editingListIndex.value = null
  editingCard.value = null
}
</script>

<style scoped></style>
