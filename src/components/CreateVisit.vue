<template>
  <div v-if="selectedPatient">
    <h2>Création d'une consultation</h2>
    <form @submit.prevent="submitForm">
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
      <button type="submit">Créer la consultation</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: {
    selectedPatient: Number
  },
  data() {
    return {
      visitData: {
        date: "",
        visitType: "",
        visitReason: "",
        comments: "",
        patientId: null
      }
    };
  },
  methods: {
    submitForm() {
      this.visitData.patientId = this.selectedPatient;
      axios.post("http://localhost:8080/api/visits", this.visitData)
          .then(response => {
            console.log("Consultation créée avec succès :", response.data);
          })
          .catch(error => {
            console.error("Erreur lors de la création de la consultation :", error);
          });
    }
  }
};
</script>

<style scoped>

</style>
