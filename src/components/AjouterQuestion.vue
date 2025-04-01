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
                console.log('Question ajoutée:', this.newQuestionTitle, this.newQuestionType);
                const question = {
                    title: this.newQuestionTitle.trim(),
                    questionType: this.newQuestionType
                };
                this.$emit('add-question', question); 
                this.newQuestionTitle = '';
                this.newQuestionType = 'text';
            }
        },
        
    }
}
</script>


<template>
    <div class="mt-3">
        <h6>Ajouter une question:</h6>
        <div class="input-group mb-3">
            <input 
                v-model="newQuestionTitle" 
                @keyup.enter="addQuestion" 
                placeholder="Ajouter un Questionnaire"
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
</template>