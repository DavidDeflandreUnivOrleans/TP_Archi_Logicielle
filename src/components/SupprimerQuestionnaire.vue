<script>
export default {
  emits: ['remove'],
  props: {
    questionnaireId: Number
  },
  methods: {
    suppr() {
      this.$emit('remove', this.questionnaireId);
    }
  }
};
</script>

<template>
  <input 
    type="button"
    class="btn btn-danger"
    value="Supprimer"
    @click="suppr"
  >
</template>
