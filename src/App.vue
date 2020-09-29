<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" v-on:update-todo="$emit('del-todo',todo.id)"></Todos>
  </div>
</template>

<script>
import Todos from './components/Todos';
import Header from './components/layout/Header';
import AddTodo from './components/AddTodo';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    Todos,
    Header,
    AddTodo,
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      axios.delete(`http://localhost:3000/api/todo/deleteTodo?id=${id}`)
      .then(res => this.todos = this.todos.filter(todo => todo.id !== id))
      .catch((err) => console.log(err));
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;

    axios.post('http://localhost:3000/api/todo/addTodo',{
      title,
      completed
    })
     .then(res => this.todos = [...this.todos, res.data])
    .catch(error => console.log(error));
    }
  },
  created() {
    axios.get('http://localhost:3000/api/todo/getTodo')
      .then(res => this.todos = res.data )
      .catch(error => console.log(error));
  }
}
</script>
<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  .btn{
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;cursor: pointer;
  }

  body {
    font-family: Arial, sans-serif;
    line-height: 1.4;
  }
</style>
