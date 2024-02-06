<template>
  <div class="container">
    <TodoForm :about="formProps" />
  </div>
</template>

<script setup lang="ts">
import TodoForm from './components/TodoForm.vue';

const formProps = {
  title: "Adam's Tasks",
  subtitle: 'Master of getting shit done'
};
</script>

<style>
@import '../node_modules/bootstrap/dist/css/bootstrap.css';
body {
  background-color: #f0f0f0;
}
</style>
