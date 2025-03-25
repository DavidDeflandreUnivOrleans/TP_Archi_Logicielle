<script>
export default {
  props: {
    questionnaire: Object
  },
  methods: {
    suppr() {
      this.$emit('remove', this.todo.id);
    },
    changerContenu() {
      const newText = prompt("Entrez le nouveau texte :");
      if (newText && newText.trim()) {
        this.$emit('replace', { id: this.todo.id, text: newText });
      }
    },
    async fetchData() {
        this.todos = null
        const res = await fetch(
            `http://localhost:5000/quiz/api/v1.0/questionnaires`
        )
        this.todos = await res.json()
    },
    mounted() {
            this.fetchData()
    }
  }
}
</script>

<template>
  <input type="button"
         class="btn btn-danger"
         value="Supprimer"
         @click="suppr">
  <input type="button"
         class="btn btn-replace"
         value="Remplacer"
         @click="changerContenu">
</template>
