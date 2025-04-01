<script>
export default{
    props: {
        questionnaire: Object,
        question: Object
    },
    data() {
        return {
        newQuestionTitle: '',
        newQuestionType: 'text'
        };
    },
    methods: {
        afficherQuestions() {
        this.showQuestions = !this.showQuestions;
        },
        supprimerQuestion() {
            this.$emit('remove-question', this.question.id); // Emit the event to remove the question
            console.log('Question supprimée:', this.question.id);
        }
    }
}
</script>

<template>
    <div class="mt-3">
        <h6>Supprimer une question:</h6>
        <div class="input-group mb-3">
            <button @click="supprimerQuestion" class="btn btn-danger" type="button">Supprimer</button>
        </div>
    </div>
</template>