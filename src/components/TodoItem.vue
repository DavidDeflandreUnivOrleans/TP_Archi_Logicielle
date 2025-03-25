<script>
export default {
  props: {
    todo: Object
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
            `https://jsonplaceholder.typicode.com/todos/`
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
