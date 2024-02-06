<template>
  <div class="mt-4 mb-4">
    <h1>{{ about.title }}</h1>
    <h4>{{ about.subtitle }} - {{ version }}</h4>
  </div>

  <div class="btn-group mb-3" role="group" aria-label="Basic example">
    <button type="button" class="btn btn-outline-success" @click="markAllDone">
      Mark All Done
    </button>
    <button type="button" class="btn btn-outline-danger" @click="removeAll">
      Remove All
    </button>
  </div>

  <form @submit.prevent="addNewTodo">
    <label for="newTodo" class="form-label strong">Create todo</label>
    <div class="mb-3">
      <div class="input-group input-group-lg">
        <input
          class="form-control"
          id="newTodo"
          placeholder="What's next?"
          v-model="newTodo"
          name="newTodo"
        />
        <button type="submit" class="btn btn-primary">Create</button>
      </div>
    </div>
  </form>

  <div v-if="todos.length === 0" class="empty-list">
    <span>Nothing added yet! 🤯</span>
  </div>
  <div v-else>
    <ul class="list-group">
      <li
        class="list-group-item d-flex flex-row justify-content-between align-items-center"
        v-for="(todo, index) in todos"
        :key="todo.id"
      >
        <span
          :class="{ completed: todo.done }"
          style="cursor: pointer"
          @click="toggleDone(todo)"
          >{{ todo.content }}
        </span>
        <button
          type="button"
          class="btn btn-outline-danger"
          @click="removeTodo(index)"
        >
          X
        </button>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { defineProps, ref } from 'vue';
import { v4 as uuidv4 } from 'uuid';
import { TodoType } from '@/models/todoModel';

interface TodoFormProps {
  about: {
    title: string;
    subtitle: string;
  };
}

const props = defineProps<TodoFormProps>();

const version = ref('v1');
const newTodo = ref('');
const todos = ref<TodoType[]>([]);

const addNewTodo = (): void => {
  if (!newTodo.value) {
    return;
  }

  todos.value.push({
    id: uuidv4(),
    done: false,
    content: newTodo.value
  });

  newTodo.value = '';
};

const toggleDone = (todo: TodoType): void => {
  todo.done = !todo.done;
};

const removeTodo = (index: number): void => {
  todos.value.splice(index, 1);
};

const markAllDone = (): void => {
  todos.value.forEach((todo) => (todo.done = true));
};

const removeAll = (): void => {
  todos.value = [];
};
</script>

<style>
label {
  font-weight: bold;
}

.completed {
  text-decoration: line-through;
  color: gray;
}

.empty-list {
  text-align: center;
}
</style>
