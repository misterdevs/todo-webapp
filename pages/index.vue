<template>
  <div
    :class="{ dark: isDarkMode }"
    class="min-h-screen w-full h-full pt-10 px-4 bg-gray-100 dark:bg-gray-900"
  >
    <ModeButton :is-dark="isDarkMode" />
    <ToDoList />
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      isDarkMode: false,
    }
  },
}
</script>
