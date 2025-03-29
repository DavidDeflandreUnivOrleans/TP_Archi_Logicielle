<script>
export default {
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
    suppr() {
      this.$emit('remove', this.questionnaire.id);
    },
    changerContenu() {
      let newText = prompt("Entrez le nouveau nom du questionnaire:", this.questionnaire.name);
      if (newText && newText.trim()) {
        this.$emit('replace', { id: this.questionnaire.id, name: newText });
      }
    },
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
  <div>
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
  </div>
</template>
