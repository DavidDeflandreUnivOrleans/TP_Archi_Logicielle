<script>
import QuestionnaireItem from './components/QuestionnaireItem.vue';
import AddQuestionnaire from './components/AddQuestionnaire.vue';

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
    addItem: function (nameQuestionnaire) {
      console.log('test');
      let name = nameQuestionnaire;
      console.log(name);
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
          console.log('succès:', data);
          return data;
        })
        .catch((error) => {
          console.error('erreur:', error);
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
          console.log('succès:', data);
          this.questionnaires = this.questionnaires.filter(todo => todo.id !== id);
        })
        .catch((error) => {
          console.error('erreur:', error);
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
          console.error('erreur dans le fetch:', error);
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
          console.log('Question ajouter:', data);
          const questionnaire = this.questionnaires.find(q => q.id === questionnaireId);
          if (questionnaire) {
            questionnaire.questions.push(data);
          }
          return data;
        })
        .catch((error) => {
          console.error(`erreur dans l'ajout:`, error);
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
  components: { QuestionnaireItem, AddQuestionnaire }
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
          </label>
          <QuestionnaireItem
            :questionnaire="questionnaire"
            @remove="removeItem"
            @replace="replaceItem"
            @add-question="addQuestionToQuestionnaire"
          ></QuestionnaireItem>
        </div>
      </li>
    </ol>
    
    <AddQuestionnaire
      @add-item='addItem'
    />
  </div>
</template>