<script>
import QuestionnaireItem from './components/QuestionnaireItem.vue';

let data = {
  questionnaires: [
    { id: 1, name: 'Faire les courses', uri: '', questions: [] },
    { id: 2, name: 'Apprendre REST', uri: '', questions: [] }
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
      let name = this.newItem.trim();
      let questionnaire = {
        id: this.questionnaires.length + 1,
        name: name,
        uri: 'http://localhost:5000/quiz/api/v1.0/questionnaires' + (this.questionnaires.length + 1),
        questions: []
      }
      const promise = this.saveQuestionnaire(questionnaire);
      console.log(promise);
      promise.then((resultat) => {
        console.log(resultat);
        this.questionnaires.push(questionnaire);
        this.newItem = '';
      });
    },
    saveQuestionnaire: function (questionnaire) {
      return fetch('http://localhost:5000/quiz/api/v1.0/questionnaires', {
        method: 'POST',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(questionnaire)
      })
        .then(response => response.json())
        .then(data => {
          console.log('Success:', data);
          return data;
        })
        .catch((error) => {
          console.error('Error:', error);
        });
    },
    replaceItem: function (item) {
      let name = item.name.trim();
      let questionnaire = {
        id: item.id,
        name: name,
        uri: 'http://localhost:5000/quiz/api/v1.0/questionnaires' + item.id,
        questions: item.questions
      }
      const promise = this.saveQuestionnaire(questionnaire);
      console.log(promise);
      promise.then((resultat) => {
        console.log(resultat);
        this.questionnaires.splice(this.questionnaires.indexOf(item), 1, questionnaire);
        this.newItem = '';
      });
    },
    removeItem: function (id) {
      return fetch("http://localhost:5000/quiz/api/v1.0/questionnaires/" + id, {
        method: 'DELETE',
      })
        .then(response => response.json())
        .then(data => {
          console.log('Success:', data);
          this.questionnaires = this.questionnaires.filter(todo => todo.id !== id);
        })
        .catch((error) => {
          console.error('Error:', error);
        });
    },
    getQuestionsForQuestionnaire: function(questionnaireId) {
      return fetch(`http://localhost:5000/quiz/api/v1.0/questionnaires/${questionnaireId}/questions`)
        .then(response => response.json())
        .then(data => {
          console.log('Questions:', data);
          const questionnaire = this.questionnaires.find(q => q.id === questionnaireId);
          if (questionnaire) {
            questionnaire.questions = data;
          }
          return data;
        })
        .catch((error) => {
          console.error('Error fetching questions:', error);
        });
    },
    addQuestionToQuestionnaire: function(questionnaireId, question) {
      const questionData = {
        title: question.title,
        questionType: question.questionType,
        questionnaire_id: questionnaireId
      };
      
      return fetch(`http://localhost:5000/quiz/api/v1.0/questionnaires/${questionnaireId}/questions`, {
        method: 'POST',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(questionData)
      })
        .then(response => response.json())
        .then(data => {
          console.log('Question added:', data);
          const questionnaire = this.questionnaires.find(q => q.id === questionnaireId);
          if (questionnaire) {
            questionnaire.questions.push(data);
          }
          return data;
        })
        .catch((error) => {
          console.error('Error adding question:', error);
        });
    }
  },
  mounted() {
    fetch('http://localhost:5000/quiz/api/v1.0/questionnaires')
      .then(response => response.json())
      .then(data => {
        console.log(data);
        this.questionnaires = data;
        this.questionnaires.forEach(questionnaire => {
          this.getQuestionsForQuestionnaire(questionnaire.id);
        });
      });
  },
  components: { QuestionnaireItem }
}
</script>

<template>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">
  
  <div class="container">
    <h2>{{ title }}</h2>
    
    <ol>
      <li v-for="questionnaire in questionnaires" :key="questionnaire.id">
        <div class="checkbox">
          <label>
             {{ questionnaire.name }} ({{ questionnaire.questions ? questionnaire.questions.length : 0 }} questions)
            <QuestionnaireItem
              :questionnaire="questionnaire"
              @remove="removeItem"
              @replace="replaceItem"
              @add-question="addQuestionToQuestionnaire"
            ></QuestionnaireItem>
          </label>
        </div>
      </li>
    </ol>
    
    <div class="input-group mb-3">
      <input 
        v-model="newItem" 
        @keyup.enter="addItem" 
        placeholder="Ajouter un Questionnaire" 
        type="text" 
        class="form-control">
      
      <button @click="addItem" class="btn btn-primary" type="button">Ajouter</button>
    </div>
  </div>
</template>
