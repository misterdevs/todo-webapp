<template>
  <div
    class="flex-row justify-center items-center text-gray-500 max-w-xl space-y-12 mx-auto"
  >
    <div class="flex-row justify-center items-center text-center">
      <h1 class="text-3xl font-bold text-gray-600">TODO WEBAPP</h1>
      <h2 class="font-light">
        The ToDo WebApp is a web-based task management application designed to
        help users organize, track, and manage various tasks and activities.
      </h2>
    </div>

    <div class="flex-row max-w-md w-full mx-auto">
      <div class="flex w-full max-w-md justify-between items-center mb-5">
        <div class="flex items-center space-x-0 sm:space-x-2">
          <input
            v-model="filterQuery"
            placeholder="Search"
            class="w-20 sm:w-1/2 p-1 border-0 border-b dark:border-gray-500 bg-transparent focus:outline-none placeholder:text-gray-500"
            @input="filter"
          />
          <select
            v-model="sortOrder"
            class="p-1 border-0 rounded-xl bg-transparent focus:outline-none"
            @change="sort"
          >
            <option value="">Sort by</option>
            <option value="taskName">Title</option>
            <option value="date">Date</option>
          </select>
          <button @click="toggleSortDirection">
            {{ sortDirection === 'asc' ? '↑' : '↓' }}
          </button>
        </div>
        <button
          class="flex justify-center font-semibold items-center space-x-1 bg-opacity-80 py-1 px-2 rounded-xl border border-gray-500 hover:border-blue-500 hover:text-blue-500"
          @click="openPopup(popupMode.Add)"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="size-4"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M12 4.5v15m7.5-7.5h-15"
            />
          </svg>
          <span class="text-sm">Add Task</span>
        </button>
      </div>
      <div v-for="(todo, index) in todos" :key="index">
        <button
          class="flex w-full px-3 md:px-6 py-4 bg-white dark:bg-gray-800 rounded-2xl mb-5"
        >
          <div class="flex w-full justify-between items-center">
            <div class="flex pr-2">
              <input
                type="checkbox"
                class="h-5 w-5 cursor-pointer appearance-none rounded-md border border-gray-200 dark:border-gray-500 checked:border-0 checked:bg-gray-900 dark:checked:bg-gray-500"
                :checked="todo.isDone"
                @change="setIsDone(index)"
              />
            </div>
            <div
              class="flex-row justify-start items-start text-start w-10/12"
              @click="editMode(index)"
            >
              <h1 class="font-semibold">{{ todo.taskName }}</h1>
              <h2 class="text-sm font-light truncate w-full">
                {{ todo.description }}
              </h2>
              <div
                class="flex sm:hidden justify-start items-center mt-2 space-x-1 text-sm font-light"
              >
                <span v-if="todo.place">{{ todo.place }},</span>
                <span v-if="todo.date"> {{ todo.date }}</span>
                <span v-if="todo.time">{{ todo.time }}</span>
              </div>
              <div
                v-if="todo.place || todo.date || todo.time"
                class="hidden justify-start items-center mt-2 sm:flex space-x-1"
              >
                <div
                  v-if="todo.place"
                  class="flex space-x-1 justify-center items-center"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke-width="1.5"
                    stroke="currentColor"
                    class="size-4"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      d="M15 10.5a3 3 0 1 1-6 0 3 3 0 0 1 6 0Z"
                    />
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      d="M19.5 10.5c0 7.142-7.5 11.25-7.5 11.25S4.5 17.642 4.5 10.5a7.5 7.5 0 1 1 15 0Z"
                    />
                  </svg>
                  <span class="text-sm font-light">{{ todo.place }}</span>
                </div>

                <div
                  v-if="todo.date"
                  class="flex space-x-1 justify-center items-center"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke-width="1.5"
                    stroke="currentColor"
                    class="size-4"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      d="M6.75 3v2.25M17.25 3v2.25M3 18.75V7.5a2.25 2.25 0 0 1 2.25-2.25h13.5A2.25 2.25 0 0 1 21 7.5v11.25m-18 0A2.25 2.25 0 0 0 5.25 21h13.5A2.25 2.25 0 0 0 21 18.75m-18 0v-7.5A2.25 2.25 0 0 1 5.25 9h13.5A2.25 2.25 0 0 1 21 11.25v7.5"
                    />
                  </svg>

                  <span class="text-sm font-light">{{ todo.date }}</span>
                </div>
                <div
                  v-if="todo.time"
                  class="flex space-x-1 justify-center items-center"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke-width="1.5"
                    stroke="currentColor"
                    class="size-4"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      d="M12 6v6h4.5m4.5 0a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
                    />
                  </svg>

                  <span class="text-sm font-light">{{ todo.time }}</span>
                </div>
              </div>
            </div>
            <div class="flex">
              <button @click="removeTodo(index)">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="size-5 hover:text-red-400"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0"
                  />
                </svg>
              </button>
            </div>
          </div>
        </button>
      </div>
      <div
        v-if="showPopup !== null"
        class="fixed inset-0 h-screen flex items-center justify-center bg-gray-800 bg-opacity-75"
      >
        <PopupForm
          v-if="showPopup === 'add'"
          title="Add Data"
          desc="Add anything what you want todo"
          :handle-submit="addTodo"
          :close-popup="closePopup"
          :form-todo="formTodo"
        />
        <PopupForm
          v-if="showPopup === 'edit'"
          title="Edit Data"
          desc="Edit anything what you want edit"
          :index="editingIndex"
          :handle-submit="updateTodo"
          :close-popup="closePopup"
          :form-todo="formTodo"
        />
      </div>
    </div>
  </div>
</template>
<script setup>
import { watchEffect } from 'vue'
watchEffect(() => {
  Boolean(JSON.parse(localStorage.getItem('todos'))) === false &&
    localStorage.setItem(
      'todos',
      JSON.stringify([
        {
          taskName: 'Technical Test TODO',
          description: 'Membangun WebApp TODO menggunakan Vue.js',
          date: '2024-05-28',
          time: '06:52',
          place: 'Cirebon',
          isDone: false,
        },
        {
          taskName: 'B Technical Test TODO',
          description: 'Ini contoh view jika tidak ada date, time dan place',
          date: '',
          time: '',
          place: '',
          isDone: true,
        },
        {
          taskName: 'C Technical Test TODO',
          description: 'Ini contoh view jika tidak ada place',
          date: '2024-06-08',
          time: '06:55',
          place: '',
          isDone: false,
        },
      ])
    )
}, [])
</script>

<script>
export default {
  data() {
    return {
      formTodo: {
        taskName: '',
        description: '',
        date: '',
        time: '',
        place: '',
        isDone: false,
      },
      todos: JSON.parse(localStorage.getItem('todos')),
      editingIndex: null,
      showPopup: null,
      popupMode: {
        Add: 'add',
        Edit: 'edit',
      },
      filterQuery: '',
      sortOrder: '',
      sortDirection: 'asc',
    }
  },
  methods: {
    addTodo() {
      if (this.formTodo.taskName.trim()) {
        this.todos.push({ ...this.formTodo })
        this.resetFormTodo()
        this.saveTodos()
        this.closePopup()
      }
    },
    updateTodo() {
      if (this.editingIndex !== null) {
        this.$set(this.todos, this.editingIndex, this.formTodo)
        this.saveTodos()
        this.closePopup()
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1)
      this.saveTodos()
    },

    setIsDone(index) {
      this.formTodo = { ...this.todos[index] }
      this.formTodo.isDone = !this.formTodo.isDone
      this.$set(this.todos, index, this.formTodo)
      this.saveTodos()
      this.resetFormTodo()
    },
    saveTodos() {
      localStorage.setItem('todos', JSON.stringify(this.todos))
    },
    editMode(index) {
      this.editingIndex = index
      this.formTodo = { ...this.todos[index] }
      this.openPopup(this.popupMode.Edit)
    },
    openPopup(popupMode) {
      this.showPopup = popupMode
    },
    closePopup() {
      this.showPopup = null
      this.resetFormTodo()
    },
    sort() {
      if (this.sortOrder.trim()) {
        this.todos = this.todos.sort((a, b) =>
          a[this.sortOrder] > b[this.sortOrder] && this.sortDirection === 'asc'
            ? 1
            : -1
        )
      } else {
        this.resetFilter()
      }
    },
    filter() {
      if (this.filterQuery.trim()) {
        console.log(this.filterQuery)
        const getTodos = JSON.parse(localStorage.getItem('todos'))
        this.todos = getTodos.filter(
          (a) =>
            a.taskName.toLowerCase().includes(this.filterQuery.toLowerCase()) ||
            a.description
              .toLowerCase()
              .includes(this.filterQuery.toLowerCase()) ||
            a.place.toLowerCase().includes(this.filterQuery.toLowerCase())
        )
      } else {
        this.resetFilter()
      }
    },
    toggleSortDirection() {
      this.sortDirection = this.sortDirection === 'asc' ? 'desc' : 'asc'
      this.sort()
    },
    resetFormTodo() {
      this.formTodo = {
        taskName: '',
        description: '',
        date: '',
        time: '',
        place: '',
        isDone: false,
      }
    },
    resetFilter() {
      this.todos = JSON.parse(localStorage.getItem('todos'))
    },
  },
}
</script>
