<template>
  <main class="p-5 font-sans">
    <div class="flex gap-5 py-5 overflow-x-auto">
      <div
        class="bg-gray-100 p-3 rounded-lg min-w-[250px] flex flex-col"
        v-for="list in lists"
        :key="list.id"
      >
        <h2 class="mb-2 font-medium">{{ list.title }}</h2>

        <Draggable :list="list.cards" group="cards">
          <template #item="{ element }">
            <div class="p-2 my-2 bg-white rounded shadow cursor-pointer">
              <span class="text-sm font-medium">{{ element.title }}</span>
              <p class="text-xs text-gray-400">{{ element.description }}</p>
            </div>
          </template>
        </Draggable>

        <button
          class="w-full p-2 mt-2 text-sm font-medium text-left text-gray-500 bg-transparent rounded hover:bg-white"
        >
          + Add card
        </button>
      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
import { reactive } from 'vue'
import Draggable from 'vuedraggable'

interface Card {
  id: number
  title: string
  description: string
}

interface List {
  id: number
  title: string
  cards: Card[]
}

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
</script>

<style scoped></style>
