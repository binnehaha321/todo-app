<template>
  <h1>Todo App</h1>
  <form @submit.prevent="addTodo">
    <label for="">New Todo</label>
    <input v-model="newTodo" name="newTodo" autocomplete="off">
    <button>Add todo</button>
  </form>

  <!-- Todos -->
  <h2>Todo list</h2>
  <ul>
    <li v-for="(todo, index) in todos" :key="index">
      <input v-if="todo.isEdit" v-model="todo.content">
      <span v-else :class="{ done: todo.done }">
        {{ todo.content }}
      </span>
      <div class="action">
        <button @click="updateTodo(index)">{{ todo.isEdit ? "Save" : "Update" }}</button>
        <button @click="doneTodo(todo)">Done</button>
        <button @click="removeTodo">Remove</button>
      </div>
    </li>
  </ul>

  <!-- Empty list -->
  <h4 v-if="todos.length === 0">Empty list</h4>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'App',

  // New feature in Vue 3
  setup() {
    // When use ref: ".value" to get value of the variable
    const newTodo = ref('');
    const todosData = JSON.parse(localStorage.getItem("todos")) || []
    const todos = ref(todosData) // array contains todos

    const addTodo = () => {
      if (newTodo.value.trim()) {
        todos.value.push({
          done: false,
          isEdit: false,
          content: newTodo.value
        });

        // Reset input
        newTodo.value = '';
      } else {
        alert("Please input your todo!")
      }
      saveData();
    }

    // Save todo
    const saveData = () => {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem("todos", storageData)
    }

    // Done todo
    const doneTodo = todo => {
      todo.done = !todo.done;
      saveData();
    }

    // Remove todo
    const removeTodo = todoId => {
      todos.value.splice(todoId, 1);
      saveData()
    }

    // Update todo
    const updateTodo = (index) => {
      todos.value[index].isEdit = !todos.value[index].isEdit;
      saveData();
    }

    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      updateTodo,
      saveData,
    }
  }
}
</script>

<style lang="scss">
$border: 2px solid rgba($color: white,
    $alpha: 0.35,
  );
$size1: 6px;
$size2: 12px;
$size3: 18px;
$size4: 24px;
$size5: 48px;
$backgroundColor: #27292d;
$textColor: white;
$primaryColor: #a0a4d9;
$secondTextColor: #1f2023;

body {
  margin: 0;
  padding: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: $backgroundColor;
  color: $textColor;

  #app {
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
    padding: 20px;

    h1 {
      font-weight: bold;
      font-size: 28px;
      text-align: center;
    }

    form {
      display: flex;
      flex-direction: column;
      width: 100%;

      label {
        font-size: 14px;
        font-weight: bold;
      }

      input,
      button {
        height: $size5;
        box-shadow: none;
        outline: none;
        padding-left: $size2;
        padding-right: $size2;
        border-radius: $size1;
        font-size: 18px;
        margin-top: $size1;
        margin-bottom: $size2;
      }

      input {
        background-color: transparent;
        border: $border;
        color: inherit;
      }
    }

    .action {
      display: flex;
      gap: 1rem;
    }

    button {
      cursor: pointer;
      background-color: $primaryColor;
      border: 1px solid $primaryColor;
      color: $secondTextColor;
      font-weight: bold;
      outline: none;
      border-radius: $size1;
    }

    h2 {
      font-size: 22px;
      border-bottom: $border;
      padding-bottom: $size1;
    }

    ul {
      padding: 10px;

      li {
        display: flex;
        justify-content: space-between;
        align-items: center;
        border: $border;
        padding: $size2 $size4;
        border-radius: $size1;
        margin-bottom: $size2;

        input {
          border-radius: 5px;
          color: #FFF;
          border: none;
          font-size: inherit;
          padding: unset;

          &:focus-visible {
            outline: none;
          }
        }

        span {
          cursor: pointer;
        }

        .done {
          text-decoration: line-through;
        }

        button {
          font-size: $size2;
          padding: $size1;
        }
      }
    }

    h4 {
      text-align: center;
      opacity: 0.5;
      margin: 0;
    }
  }
}
</style>