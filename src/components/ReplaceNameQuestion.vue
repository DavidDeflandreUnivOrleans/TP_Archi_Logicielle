<script>
export default {
  props: {
    questionnaire: Object,
    question: Object
  },
  data() {
    return {
      newTitle: '',
      newQuestionType: 'text'
    };
  },
  methods: {
    replaceQuestion() {
      if (this.newTitle.trim()) {
        this.$emit('replace', { id: this.question.id, title: this.newTitle, questionType: this.newQuestionType });
        console.log('Question modifiée:', this.newTitle, this.newQuestionType);
        this.newQuestionType = 'text';
        this.newTitle = '';
      }
    }
  }
};
</script>

<template>
  <div class="input-group mb-3">
    <input 
      v-model="newTitle" 
      @keyup.enter="replaceItem" 
      placeholder="Modifier le nom du questionnaire"
      type="text"
      class="form-control">
    <select v-model="newQuestionType" class="form-select">
      <option value="text">Texte</option>
      <option value="choice">Choix multiple</option>
      <option value="boolean">Oui/Non</option>
    </select>
    <button @click="replaceQuestion" class="btn btn-primary" type="button">Modifier</button>
  </div>
</template>