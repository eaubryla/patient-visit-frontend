<template>
  <div v-if="showUpdateVisit">
    <h2>Mis à jour d'une consultation</h2>
    <form @submit.prevent="submitForm">
      <input hidden="hidden" id="visitId" v-model="visitData.visitId" required>
      <input hidden="hidden" id="patientId" v-model="visitData.patientId" required>
      <div>
        <label for="date">Date :</label>
        <input type="date" id="date" v-model="visitData.date" required>
      </div>
      <div>
        <label for="visitType">Type de consultation :</label>
        <select id="visitType" v-model="visitData.visitType" required>
          <option value="home">Domicile</option>
          <option value="office">Cabinet</option>
        </select>
      </div>
      <div>
        <label for="visitReason">Raison de la consultation :</label>
        <select id="visitReason" v-model="visitData.visitReason" required>
          <option value="first">Première consultation</option>
          <option value="recurring">Consultation récurrente</option>
          <option value="urgent">Urgence</option>
        </select>
      </div>
      <div>
        <label for="comments">Commentaires :</label>
        <textarea id="comments" v-model="visitData.comments"></textarea>
      </div>
      <button type="submit">Modifier la consultation</button>
      <button @click="cancel">Annuler</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: {
    selectedVisit: Object,
    showUpdateVisit: Boolean
  },
  data() {
    return {
      visitData: {
        date: this.selectedVisit.date,
        visitType: this.selectedVisit.visitType,
        visitReason: this.selectedVisit.visitReason,
        comments: this.selectedVisit.comments,
        patientId: this.selectedVisit.patientId,
        visitId: this.selectedVisit.visitId
      }
    };
  },
  methods: {
    submitForm() {
      axios.put(`http://localhost:8080/api/visits/${this.selectedVisit.visitId}`, this.visitData)
          .then(response => {
            console.log("Consultation modifiée avec succès :", response.data);
            this.$emit("update", response.data);
          })
          .catch(error => {
            console.error("Erreur lors de la modification de la consultation :", error);
          });
    },
    cancel() {
      this.$emit("cancel");
    }
  }
};
</script>

<style scoped>

</style>
