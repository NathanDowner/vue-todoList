<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
  import Todos from '../components/Todos';
  import AddTodo from '../components/AddTodo';
  import axios from 'axios';

  export default {
    name: 'Home',
    components: {
      Todos,
      AddTodo
    },
    data() {
      return {
        todos: [],
        url: 'https://jsonplaceholder.typicode.com/todos'
      }
    },
    created () {
      fetch(this.url + '?_limit=5')
        .then(resp => resp.json())
        .then(json => this.todos = json)
        .catch(err => console.log(err));
    },
    methods: {
      deleteTodo(id) {
        axios.delete(this.url + `/${id}`)
        .then(res => this.todos = this.todos.filter(td => td.id !== id))
        .catch(e => console.log(e));
      },
      addTodo(newTodo) {
        const { title, created } = newTodo;
        
        axios.post(this.url, {
          title,
          created
        }).then(res => this.todos = [...this.todos, res.data])
          .catch(err => console.log(err));
      }
    }
  }
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }

  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }

  .btn:hover {
    background: #666;
  }
</style>
