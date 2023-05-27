<template>
  <div
    class="item"
    @mouseover="showDelBtn = true"
    @mouseleave="showDelBtn = false"
  >
    <div class="body">
      <n-checkbox
        checked-value="true"
        unchecked-value="false"
        @update:checked="toggleIsDone"
      />
      <h3 :class="{ isDone: isChecked }">{{ content }}</h3>
    </div>

    <n-button v-if="showDelBtn" type="error" @click="deleteVTodo"
      >Delete</n-button
    >
  </div>
</template>

<script setup>
import { NCheckbox, NButton } from "naive-ui";

import { ref, toRefs } from "vue";

import TodoItem from "../TodoItem";

const emit = defineEmits(["toggleDone", "deleteTodo"]);

const props = defineProps({
  content: String,
  vTodo: TodoItem,
});
const { content, vTodo } = toRefs(props);

const showDelBtn = ref(false);
const isChecked = ref(false);

const toggleIsDone = (v) => {
  isChecked.value = v === "true" ? true : false;
  emit("toggleDone", vTodo.value, isChecked.value);
};

const deleteVTodo = (v) => {
  emit("deleteTodo", vTodo.value);
};
</script>

<style>
.item {
  display: flex;
  align-items: center;
  margin: 0 auto;
  justify-content: space-between;
  background: #fff;
  border: 1px solid #ccc;
  /* border-radius: 5px; */
  padding: 10px;
}
.isDone {
  text-decoration: line-through;
  color: #888;
}
.body {
  display: flex;
}
.body h3 {
  margin-left: 50px;
}
</style>