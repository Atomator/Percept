<template>
  <div class = "container">
    <h1 class="text-primary">Demo Task List</h1>
    <router-link to="/dashboard">
        <button class="btn btn-dark">Return to Dashboard</button>
    </router-link>
    <div style="margin: 50px;">
      <form>
        <div class="form-group">
          <label>
            <h5>New Todo:</h5>
            <input v-model="newTodo" class="form-control" type="text"/>
            <button class="btn btn-primary"  style="margin: 15px;" type="submit" @click.prevent="addTodo()">Add</button>
          </label>
        </div>
      </form>
    </div>
    <div>
    <ul class="list-group">
      <li class="list-group-item" v-for="todo in todos" :key="todo.id">
        {{todo.name}}
        <button style="float: right;" class="btn btn-outline-danger" @click="deleteNote(todo.id)"> Delete </button>
     </li>
    </ul>
    </div>
  </div>
</template>

<script>
import { todosCollection } from '../store/firebase';

export default {
  name: 'DemoTask',
  data () {
    return {
      newTodo: '',
      todos: []
    }
  },
  firestore() {
    return {
      todos: todosCollection.orderBy('createdAt', 'desc')
    }
  },
  methods: {
    addTodo() {
      todosCollection.add({
        name: this.newTodo,
        createdAt: new Date()
      })
      .then(function(docRef) {
        console.log("Document written with ID: ", docRef.id);
      })
      .catch(function(error) {
        console.log("Error adding document: ", error);
      });

      this.newTodo = '';
    },
    deleteNote (id) {
      todosCollection.doc(id).delete()
    }

  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
