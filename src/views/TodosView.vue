<script setup>
import TodoCreator from "../components/TodoCreator.vue";
import TodoItem from "../components/TodoItem.vue";
import { ref, watch, computed } from "vue";
import { uid } from "uid";
import { Icon } from "@iconify/vue";

const todoList = ref([]);
watch(
  todoList,
  () => {
    saveTodoList();
  },
  {
    deep: true,
  }
);

const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted);
});

function createTodo(todo) {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEdited: null,
  });
}

function toggleTodoComplete(todoIndex) {
  todoList.value[todoIndex].isCompleted =
    !todoList.value[todoIndex].isCompleted;
}

function toggleEditTodo(todoIndex) {
  todoList.value[todoIndex].isEdited = !todoList.value[todoIndex].isEdited;
}

function updateTodo(todoValue, todoIndex) {
  todoList.value[todoIndex].todo = todoValue;
}

function deleteTodo(todoId) {
  todoList.value = todoList.value.filter((todo) => todo.id !== todoId);
}

function saveTodoList() {
  localStorage.setItem("todoList", JSON.stringify(todoList.value));
}

function fetchTodoList() {
  const savedTodoList = JSON.parse(localStorage.getItem("todoList"));
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
}

fetchTodoList();
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem
        v-for="(todo, index) in todoList"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </ul>
    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" width="22" />
      <span>You have no todos to complete! Add one!</span>
    </p>
    <p class="todos-msg" v-if="todoCompleted && todoList.length > 0">
      <Icon icon="noto-v1:party-popper" width="22" />
      <span>You have completed all of your todos!</span>
    </p>
  </main>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
