<script>
export default {
  emits: ['add-question'],
  props: {
    questionnaire: Object
  },
  data() {
    return {
      showQuestions: false,
      newQuestionTitle: '',
      newQuestionType: 'text'
    }
  },
  methods: {
    afficherQuestions() {
      this.showQuestions = !this.showQuestions;
    },
    addQuestion() {
      if (this.newQuestionTitle.trim()) {
        const newQuestion = {
          title: this.newQuestionTitle,
          questionType: this.newQuestionType,
          questionnaire_id: this.questionnaire.id
        };
        this.$emit('add-question', this.questionnaire.id, newQuestion);
        this.newQuestionTitle = '';
      }
    }
  }
}
</script>

<template>
    <input type="button"
           class="btn btn-info"
           value="Afficher Questions"
           @click="afficherQuestions">
           
    <div v-if="showQuestions" class="mt-3">
      <h5>Questions:</h5>
      <ul class="list-group">
        <li v-if="questionnaire.questions && questionnaire.questions.length === 0" class="list-group-item">
          Aucune question pour ce questionnaire
        </li>
        <li v-for="question in questionnaire.questions" :key="question.id" class="list-group-item">
          <strong>{{ question.title }}</strong> (Type: {{ question.questionType }})
        </li>
      </ul>
      
      <div class="mt-3">
        <h6>Ajouter une question:</h6>
        <div class="input-group mb-3">
          <input 
            v-model="newQuestionTitle" 
            @keyup.enter="addQuestion"
            placeholder="Titre de la question" 
            type="text" 
            class="form-control">
          
          <select v-model="newQuestionType" class="form-select">
            <option value="text">Texte</option>
            <option value="choice">Choix multiple</option>
            <option value="boolean">Oui/Non</option>
          </select>
          
          <button @click="addQuestion" class="btn btn-success" type="button">Ajouter</button>
        </div>
      </div>
    </div>
</template>
