<script>
import TodoItem from './components/TodoItem.vue';

let data = {
  questionnaires: [
    { id: 1, name: 'Faire les courses', uri: 'http://localhost:5000/quiz/api/v1.0/questionnaires/1' },
    { id: 2, name: 'Apprendre REST', uri : 'http://localhost:5000/quiz/api/v1.0/questionnaires/2' }
  ],
  title: 'Mes questionnaires',
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
        this.questionnaires.push({
          id: Date.now(),
          text: text,
          checked: false
        });
        this.newItem = '';
      }
    },
    removeItem(todoId) {
      this.questionnaires = this.questionnaires.filter(todo => todo.id !== todoId);
    },
    replaceItem({ id, text }) {
      const todo = this.questionnaires.find(todo => todo.id === id);
      if (todo) {
        todo.text = text;
      }
    }

  },
  mounted() {
    fetch('http://localhost:5000/quiz/api/v1.0/questionnaires')
      .then(response => response.json())
      .then(data => {
        console.log(data);
        this.questionnaires = data;
      });
  },
  components: { TodoItem }
}
</script>

<template>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">
  
  <div class="container">
    <h2>{{ title }}</h2>
    
    <ol>
      <li v-for="questionnaire in questionnaires" :class="{ 'alert alert-success': questionnaire.checked }" :key="questionnaire.id">
        <div class="checkbox">
          <label>
             {{ questionnaire.name }}
            <TodoItem
              :todo="questionnaire"
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
