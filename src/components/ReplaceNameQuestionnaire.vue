<script>
export default {
  emits: ['replace'],
  props: {
    questionnaire: Object
  },
  methods: {
    changerContenu() {
      let newText = prompt("Entrez le nouveau nom du questionnaire:", this.questionnaire.name);
      if (newText && newText.trim()) {
        this.$emit('replace', { 
          id: this.questionnaire.id, 
          name: newText.trim() 
        });
      }
    }
  }
};
</script>

<template>
  <input 
    type="button"
    class="btn btn-replace"
    value="Remplacer"
    @click="changerContenu"
  >
</template>
