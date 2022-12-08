<template>
  <div id="app">

    <form v-on:submit="postAPI()">
      <input type="text" v-model="nameIs"> <br> <br>
      <input type="text" v-model="infoIs"> <br> <br>
      <button type="submit">Submit</button>
      <br>
    </form>
    <br>
    <br>
    <br>
    <br>

    <table align="center">

      <tr>
        <td>ID</td>
        <td>Title</td>
        <td>Update</td>
        <td>Delete</td>
      </tr>

      <tr v-for="todo of todos" v-bind:key="todo.id">
        <td>{{todo.id}}</td>
        <td><input type="text" v-model="todo.name"/></td>
        <td><input type="text" v-model="todo.info"/></td>
        <td><button v-on:click="updateAPI(todo.id,todo.name,todo.info)">Update</button></td>
        <td><button v-on:click="deleteAPI(todo.id)">Delete</button></td>
      </tr>

    </table>
  </div>
</template>

<script>
import axios from 'axios'
const baseUrl = 'http://localhost:3000/todos/';

export default {
  name: 'App',
  data() {
    return {
      todos: [],
      nameIs: '',
      infoIs: '',
    }
  },
  methods: {
    async getAPI() {
      await axios.get(baseUrl).then(response => {
        this.todos = response.data;
      }).catch(error => {
        console.log(error)
      })
    },
    async postAPI() {
      await axios.post(baseUrl, {name:this.nameIs,info:this.infoIs,}).then(response => {
        console.log(response); this.getAPI()}).catch(error => {console.log(error)
        })
    },
    async deleteAPI(id) {
      await axios.delete(baseUrl+id).then(response => {
        console.log(response); this.getAPI()}).catch(error => {console.log(error)
        })
    },
    async updateAPI(id,name,info) {
      await axios.put(baseUrl+id,{name:name,info:info}).then(response => {
        console.log(response); this.getAPI()}).catch(error => {console.log(error)
        })
    },
  },
  mounted() {
    this.getAPI();
  }
}
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
</style>
