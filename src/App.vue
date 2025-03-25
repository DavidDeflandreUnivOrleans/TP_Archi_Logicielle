<script>
import TodoItem from './components/TodoItem.vue';

let data = {
  todos: [
    { id: 1, text: 'Faire les courses', checked: true },
    { id: 2, text: 'Apprendre REST', checked: false }
  ],
  title: 'Mes tâches',
  newItem: ''
};

export default {
  data() {
    return data;
  },
  methods: {
    addItem: function () {
      let text = this.newItem.trim();
      if (text) {
        this.todos.push({
          id: Date.now(),
          text: text,
          checked: false
        });
        this.newItem = '';
      }
    },
    removeItem(todoId) {
      this.todos = this.todos.filter(todo => todo.id !== todoId);
    },
    replaceItem({ id, text }) {
      const todo = this.todos.find(todo => todo.id === id);
      if (todo) {
        todo.text = text;
      }
    }
  },
  components: { TodoItem }
}
</script>

<template>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">
  
  <div class="container">
    <h2>{{ title }}</h2>
    
    <ol>
      <li v-for="todo in todos" :class="{ 'alert alert-success': todo.checked }" :key="todo.id">
        <div class="checkbox">
          <label>
            <input type="checkbox" v-model="todo.checked"> {{ todo.text }}
            <TodoItem
              :todo="todo"
              @remove="removeItem"
              @replace="replaceItem"
            ></TodoItem>
          </label>
        </div>
      </li>
    </ol>
    
    <div class="input-group mb-3">
      <input 
        v-model="newItem" 
        @keyup.enter="addItem" 
        placeholder="Ajouter une tâche à la liste" 
        type="text" 
        class="form-control">
      
      <button @click="addItem" class="btn btn-primary" type="button">Ajouter</button>
    </div>
  </div>
</template>
