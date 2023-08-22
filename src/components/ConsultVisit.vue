<template>
  <div v-if="selectedPatient">
    <div>
      <h3>Visualisation des consultation</h3>
      <table>
        <thead>
        <tr>
          <th>Date</th>
          <th>Type</th>
          <th>Raison</th>
          <th>Commentaire</th>
          <th>Actions</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="visit in patientVisits" :key="visit.visitId">
          <td>{{ visit.date }}</td>
          <td>{{ visit.visitType }}</td>
          <td>{{ visit.visitReason }}</td>
          <td>{{ visit.comments }}</td>
          <td>
            <button @click="editVisit(visit)">Modifier</button>
          </td>
        </tr>
        </tbody>
      </table>
    </div>
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
      patientVisits: []
    };
  },
  mounted() {
    this.fetchPatientVisits(this.selectedPatient);
  },
  methods: {
    fetchPatientVisits(selectedPatient) {
      axios.get(`http://localhost:8080/api/visits/patient/${selectedPatient}`)
          .then(response => {
            this.patientVisits = response.data;
          })
          .catch(error => {
            console.error("Erreur lors de la récupération des consultation du patient :", error);
          });
    },
    editVisit(visit) {
      // Gérez la modification
      console.log("Modifier la consultation :", visit);
    }
  }
};
</script>

<style scoped>

</style>
