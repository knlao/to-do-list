<template>
  <div class="header">
    <h1>My To-do List ({{ filterDoneTodo().length }}/{{ todos.length }})</h1>
  </div>
  <div class="inputField">
    <n-input
      maxlength="30"
      show-count
      clearable
      autosize
      style="min-width: 33%"
      size="large"
      placeholder="new to-do"
      autocorrect="false"
      v-model:value="todoInput"
      @keyup.enter="addNewTodo"
    />
    <n-button type="primary" class="addBtn" @click="addNewTodo"
      >Add New Todo</n-button
    >
  </div>

  <div class="todoList">
    <div v-for="todo in todos" :key="todo">
      <TodoItemVue
        :vTodo="todo"
        :content="todo.content"
        @toggleDone="toggleTodoIsDone"
        @deleteTodo="deleteVTodo"
      />
    </div>
  </div>
</template>

<script setup>
import { NInput, NButton } from "naive-ui";
import TodoItemVue from "./components/TodoItem.vue";

import TodoItem from "./TodoItem";

import { ref } from "vue";

const todoInput = ref("");

let todoTwo = new TodoItem("buy milk");
let todoOne = new TodoItem("do homework");

const todos = ref([todoOne, todoTwo]);

const addNewTodo = () => {
  let ok = true;
  if (todoInput.value.trim() === "" || todoInput.value === null) {
    alert("Todos cannot be null");
    ok = false;
  }

  todos.value.forEach((todo) => {
    if (todo.content === todoInput.value) {
      alert("Todos cannot be repeated");
      ok = false;
    }
  });

  if (ok) {
    let tempTodo = new TodoItem(todoInput.value);

    todoInput.value = "";
    todos.value.push(tempTodo);
  }
};

const filterDoneTodo = () => {
  let filteredTodo = todos.value.filter((todo) => {
    return todo.done;
  });
  return filteredTodo;
};

const filterNotDoneTodo = () => {
  let filteredTodo = todos.value.filter((todo) => {
    return !todo.done;
  });
  return filteredTodo;
};

const toggleTodoIsDone = (delTodo, status) => {
  todos.value[todos.value.indexOf(delTodo)].done = status;
};

const deleteVTodo = (delTodo) => {
  todos.value.splice(todos.value.indexOf(delTodo), 1);
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.todoList {
  max-width: 480px;
  margin: 0 auto;
  margin-top: 25px;
  box-shadow: 0 5px 30px rgb(0 0 0 / 20%);
}
.inputField {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
