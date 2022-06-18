<template>
  <ul class="pt-6">
    <li
      v-for="(todo, index) in todos"
      :key="index"
      class="mx-auto p-4 mb-2 border rounded-md border-black"
    >
      <div class="flex justify-between items-center text-2xl">
        <div class="flex-wrap w-4/5">
          <div v-if="todo.isDone">
            <del>{{ todo.activity }}</del>
          </div>
          <div v-else>{{ todo.activity }}</div>
        </div>
        <div class="text-center w-1/5 lg:space-x-4">
          <button
            class="py-2 px-4 bg-blue-600 rounded-md text-white hover:bg-blue-800"
            @click="done(index)"
          >
            ✓
          </button>
          <button
            class="mt-2 lg:mt-0 py-2 px-4 bg-red-600 rounded-md text-white hover:bg-red-800"
            @click="deleteTodo(index)"
          >
            X
          </button>
        </div>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      default: [],
    },
  },
  setup(props, { emit }) {
    const deleteTodo = (index) => {
      emit("deleteTodo", index);
    };

    const done = (index) => {
      emit("doneTodo", index);
    };
    return {
      deleteTodo,
      done,
    };
  },
};
</script>

<style></style>
