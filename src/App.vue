<template>
  <div
    class="container mx-auto"
  >
    <div class="text-bold text-4xl mx-4 my-4 border rounded-md p-2 lg:p-6 border-black">
      <h2 class="text-center">SIMPLE TO DO APP</h2>
      <div class="lg:flex flex-wrap text-center justify-center items-center pt-4">
        <input
          v-model="todo"
          type="text"
          class="p-3 border border-black rounded-md text-xl w-auto"
          @keyup.enter="add"
        />
        <div class="">
          <button
          class="mt-4 lg:mt-0 py-2 px-4 bg-green-600 rounded-md text-white lg:ml-6 hover:bg-green-800"
          @click="add"
        >
          Add
        </button>
        </div>
      </div>
      <list :todos="list" @doneTodo="doneTodo" @deleteTodo="deleteTodo" />
      <small>Total TO DO : {{ totalTodo }}</small>
    </div>
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
