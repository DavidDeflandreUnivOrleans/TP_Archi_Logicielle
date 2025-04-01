<script>
import AjouterQuestion from './AjouterQuestion.vue';
import ReplaceNameQuestion from './ReplaceNameQuestion.vue';
import SupprimerQuestion from './SupprimerQuestion.vue';

export default {
  emits: ['add-question', 'edit-question', 'remove-question'],
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
    addQuestion(question) {
      this.$emit('add-question', this.questionnaire.id, question); // Ajoute l'ID du questionnaire
    },
    replaceItem(updatedQuestion) {
      this.$emit('edit-question', this.questionnaire.id, updatedQuestion); // Transmet l'ID du questionnaire et la question mise à jour
    },
    removeQuestion(questionId) {
      this.$emit('remove-question', this.questionnaire.id, questionId); // Inclut l'ID du questionnaire
    }
  },
  components: {
    AjouterQuestion, ReplaceNameQuestion, SupprimerQuestion
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
          <div class="checkbox-actions">
            <ReplaceNameQuestion :question="question" @replace="replaceItem" />
            <SupprimerQuestion :question="question" @remove-question="removeQuestion"/>
          </div>
        </li>
      </ul>
      <AjouterQuestion :questionnaire="questionnaire" @add-question="addQuestion"/>
    </div>
</template>