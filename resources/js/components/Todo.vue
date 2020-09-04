<template>
  <div>
    <div v-for="(todo,index) in todos" :key="index">
      <todo-item :todo="todo" @del-todo="$emit('del-todo',todo.id)"></todo-item>
    </div>
  </div>
</template>
<script>
import TodoItem from "./TodoItem";
export default {
  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
  components: {
    "todo-item": TodoItem,
  },
};
</script>