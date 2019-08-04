<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="delTodo" />
  </div>
</template>

<script>
import Todos from "./components/Todos";
import Header from "./components/layout/Header";
import AddTodo from "./components/AddTodo";

export default {
  name: "app",
  components: {
    Header,
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    delTodo(id) {
      fetch(`https://jsonplaceholder.typicode.com/todos/${id}`, {
        method: "DELETE"
      })
        .then(response => response.json())
        .then(res => {
          this.todos = this.todos.filter(todo => {
            return todo.id !== id;
          });
        })
        .catch(error => {
          console.error("Error:", error);
        });
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;

      fetch("https://jsonplaceholder.typicode.com/todos", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({ title, completed })
      })
        .then(response => response.json())
        .then(response => {
          console.log("RESPONSE==>>", response);
          this.todos = [...this.todos, response];
        })
        .catch(err => console.log("Error:", err));
    }
  },
  created() {
    fetch("https://jsonplaceholder.typicode.com/todos?_limit=5")
      .then(response => response.json())
      .then(json => {
        this.todos = json;
      });
    //http://jsonplaceholder.typicode.com/todos
  }
};
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
  background: #555;
  display: inline-block;
  border: none;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}
.btn:hover {
  background: #666;
}
</style>
