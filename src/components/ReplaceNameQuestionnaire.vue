<script>
export default {
  emits: ['replace'],
  props: {
    questionnaire: Object
  },
  data() {
    return {
      isEditing: false,
      newName: this.questionnaire.name
    };
  },
  methods: {
    startEditing() {
      this.isEditing = true;
    },
    saveName() {
      if (this.newName.trim() && this.newName !== this.questionnaire.name) {
        this.$emit('replace', { 
          id: this.questionnaire.id, 
          name: this.newName.trim() 
        });
        this.isEditing = false;
      }
    },
    cancelEditing() {
      this.isEditing = false;
      this.newName = this.questionnaire.name;
    }
  }
};
</script>

<template>
    <div v-if="isEditing">
      <input 
        v-model="newName"
        type="text" 
        class="form-control"
        @keyup.enter="saveName" 
        @blur="saveName"
      />
      <button class="btn btn-success" @click="saveName">Enregistrer</button>
      <button class="btn btn-secondary" @click="cancelEditing">Annuler</button>
    </div>
    <div v-else>
      <button 
        class="btn btn-replace"
        @click="startEditing"
      >
        Modifier le nom
      </button>
    </div>
</template>