<!-- eslint-disable vue/no-mutating-props -->
<template>
  <div
    class="max-w-md px-6 p-5 bg-gray-100 dark:bg-gray-900 rounded-3xl shadow-lg"
  >
    <div class="flex mb-8 justify-between items-start">
      <div class="flex-row">
        <h2 class="text-xl font-bold text-gray-600">{{ title }}</h2>
        <span class="text-xs">{{ desc }}</span>
      </div>
      <button @click="closePopup">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="size-6 text-gray-500 hover:text-red-500"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="m9.75 9.75 4.5 4.5m0-4.5-4.5 4.5M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
          />
        </svg>
      </button>
    </div>
    <form class="flex-row mb-4" @submit.prevent="handleSubmit">
      <div class="flex-row mb-2">
        <label class="text-sm font-light">Todo Name</label>
        <input
          v-model="formTodo.taskName"
          class="w-full p-1 border-0 border-b-2 focus:outline-none bg-gray-100 dark:bg-gray-900 dark:border-gray-500"
        />
      </div>
      <div class="flex-row mb-2">
        <label class="text-sm font-light">Description</label>
        <textarea
          v-model="formTodo.description"
          class="w-full p-1 border-0 border-b-2 focus:outline-none bg-gray-100 dark:bg-gray-900 dark:border-gray-500"
        ></textarea>
      </div>
      <div class="flex space-x-4 mb-2">
        <div class="flex-row w-full">
          <label class="text-sm font-light">Date</label>
          <input
            v-model="formTodo.date"
            type="date"
            class="w-full p-1 border-0 border-b-2 focus:outline-none bg-gray-100 dark:bg-gray-900 dark:border-gray-500"
          />
        </div>
        <div class="flex-row w-1/3">
          <label class="text-sm font-light">Time</label>
          <input
            v-model="formTodo.time"
            type="time"
            class="w-full p-1 border-0 border-b-2 focus:outline-none bg-gray-100 dark:bg-gray-900 dark:border-gray-500"
          />
        </div>
      </div>
      <div class="flex-row mb-2">
        <label class="text-sm font-light">Place</label>
        <input
          v-model="formTodo.place"
          class="w-full p-1 border-0 border-b-2 focus:outline-none bg-gray-100 dark:bg-gray-900 dark:border-gray-500"
        />
      </div>

      <div class="flex w-full mt-5">
        <button
          type="submit"
          class="w-full rounded-2xl border border-gray-500 text-gray-500 hover:border-blue-500 hover:text-blue-500 py-2 px-4"
        >
          {{ isUpdateMode ? 'Update' : 'Add' }}
        </button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    index: { type: Number, required: false, default: null },
    title: { type: String, required: true },
    desc: { type: String, required: true },
    handleSubmit: { type: Function, required: true },
    closePopup: { type: Function, required: true },
    formTodo: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      isUpdateMode: this.index !== null,
    }
  },
}
</script>
