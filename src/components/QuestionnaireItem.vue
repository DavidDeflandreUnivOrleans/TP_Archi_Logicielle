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
    addQuestion(type) {
      
      if (this.newQuestionTitle.trim()) {
        const newQuestion = {
          title: this.newQuestionTitle,
          questionType: type,
          questionnaire_id: this.questionnaire.id
        };
        this.$emit('add-question', this.questionnaire.id, newQuestion);
        this.newQuestionTitle = '';
      }
    },
    supprQuestion(questionId) {
      this.$emit('remove-question', this.questionnaire.id, questionId);
    },
    changerContenuQuestion(questionId) {
      let newText = prompt("Entrez le nouveau nom de la question:", this.questionnaire.questions.find(q => q.id === questionId).title);
      let newQuestionType = prompt("Entrez le nouveau type de question (text, choice, boolean):", this.questionnaire.questions.find(q => q.id === questionId).questionType);
      if (newText && newText.trim() && newQuestionType) {
        this.$emit('replace-question', this.questionnaire.id, questionId, { title: newText, questionType: newQuestionType });
      }
    }
  }
}
</script>

<template>
  <div>
    <strong>{{ questionnaire.name }}</strong>
    <span class="badge bg-secondary">{{ questionnaire.questions ? questionnaire.questions.length : 0 }} questions</span>
    <input type="button"
           class="btn btn-danger"
           value="Supprimer"
           @click="suppr">
    <input type="button"
           class="btn btn-replace"
           value="Remplacer"
           @click="changerContenu">
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
          <input type="button" class="btn btn-danger btn-sm float-end" value="Supprimer" @click="supprQuestion(question.id)">
          <input type="button" class="btn btn-replace btn-sm float-end me-2" value="Remplacer" @click="changerContenuQuestion(question.id)">
        </li>
      </ul>
      
      <div class="mt-3">
        <h6>Ajouter une question:</h6>
        <div class="input-group mb-3">
          <input 
            v-model="newQuestionTitle"
            @keyup.enter="addQuestion(newQuestionType)" 

            placeholder="Titre de la question" 
            type="text" 
            class="form-control">
          <select v-model="newQuestionType" class="form-select">
            <option value="text">Texte</option>
            <option value="choice">Choix multiple</option>
            <option value="boolean">Oui/Non</option>
          </select>
          <button @click="addQuestion(newQuestionType)" class="btn btn-success" type="button">Ajouter</button>
        </div>
      </div>
    </div>
</template>
