<template>
  <div
    class="container mx-auto text-bold text-4xl my-4 border rounded-md p-6 border-black"
  >
    <h2 class="text-center">SIMPLE TODO APP</h2>
    <div class="flex justify-center items-center pt-4">
      <input
        v-model="todo"
        type="text"
        class="p-4 border border-black rounded-md text-xl w-80"
        @keyup.enter="add"
      />
      <button
        class="py-2 px-4 bg-green-600 rounded-md text-white ml-6 hover:bg-green-800"
        @click="add"
      >
        Add
      </button>
    </div>
    <list :todos="list" @doneTodo="doneTodo" @deleteTodo="deleteTodo" />
    <small>Total TODO : {{ totalTodo }}</small>
  </div>
</template>

<script>
import List from "./components/List.vue";
import { ref, reactive, toRefs, onMounted, computed } from "vue";
export default {
  components: { List },
  setup() {
    const todo = ref("");
    const todos = reactive({
      list: [],
    });

    onMounted(() => {
      const items = localStorage.getItem("todos");
      todos.list = items ? JSON.parse(items) : [];
    });

    const totalTodo = computed(() => {
      return todos.list.length;
    });

    const add = () => {
      todos.list.unshift({
        activity: todo.value,
        isDone: false,
      });
      todo.value = "";
      saveToLocalStorage();
    };

    const doneTodo = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = !item.isDone;
        }
        return item;
      });
      saveToLocalStorage();
    };

    const deleteTodo = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index != todoIndex) {
          return item;
        }
      });
      saveToLocalStorage();
    };

    const saveToLocalStorage = () => {
      localStorage.setItem("todos", JSON.stringify(todos.list));
    };
    return {
      todo,
      ...toRefs(todos),
      totalTodo,
      add,
      doneTodo,
      deleteTodo,
    };
  },
};
</script>

<style></style>
