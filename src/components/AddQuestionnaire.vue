<script>
export default {
  emits: ['add-item'],
  props: {
    questionnaire: Object
  },
  data() {
    return {
      newItem: ''
    };
  },
  methods: {
    addItem() {
      if (this.newItem.trim()) {
        this.$emit('add-item', this.newItem.trim());
        this.newItem = '';
      }
    }
  }
};
</script>

<template>
  <div class="input-group mb-3">
    <input 
      v-model="newItem" 
      @keyup.enter="addItem" 
      placeholder="Ajouter un Questionnaire" 
      type="text"
      class="form-control">
    
    <button @click="addItem" class="btn btn-primary" type="button">Ajouter</button>
  </div>
</template>