<template>
  <div v-if="selectedPatient">
    <h2>Création d'une Visite</h2>
    <form @submit.prevent="submitForm">
      <div>
        <label for="date">Date :</label>
        <input type="date" id="date" v-model="visitData.date" required>
      </div>
      <div>
        <label for="visitType">Type de Visite :</label>
        <select id="visitType" v-model="visitData.visitType" required>
          <option value="home">Domicile</option>
          <option value="office">Cabinet</option>
        </select>
      </div>
      <div>
        <label for="visitReason">Raison de la Visite :</label>
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
      <button type="submit">Créer la Visite</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: {
    selectedPatient: Object
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
      console.log(this.selectedPatient);
      this.visitData.patientId = this.selectedPatient.patientId;
      axios.post("http://localhost:8080/api/visits", this.visitData)
          .then(response => {
            console.log("Visite créée avec succès :", response.data);
          })
          .catch(error => {
            console.error("Erreur lors de la création de la visite :", error);
          });
    }
  }
};
</script>

<style scoped>

</style>
