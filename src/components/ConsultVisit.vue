<template>
  <div v-if="selectedPatient">
    <div>
      <h2>Visualisation des consultations</h2>
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
        <tr v-for="visit in patientVisits" :key="visit.visitId" v-bind="selectedVisit">
          <td>{{ visit.date }}</td>
          <td>{{ visit.visitType }}</td>
          <td>{{ visit.visitReason }}</td>
          <td>{{ visit.comments }}</td>
          <td>
            <button @click="editVisit(visit)" >Modifier</button>
          </td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
  <UpdateVisit v-if="showUpdateVisit" :selectedVisit="selectedVisit" :showUpdateVisit="showUpdateVisit" @update="updateVisitData" @cancel="cancel"/>
</template>

<script>
import axios from "axios";
import UpdateVisit from "@/components/UpdateVisit.vue";

export default {
  components: {
    UpdateVisit
  },
  props: {
    selectedPatient: Number
  },
  data() {
    return {
      patientVisits: [],
      showUpdateVisit: false,
      selectedVisit: null
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
      this.selectedVisit = visit;
      this.showUpdateVisit = true;
    },
    cancel() {
      this.showUpdateVisit = false;
    },
    updateVisitData(updatedVisit) {
      // Recherchez et mettez à jour la visite modifiée dans le tableau
      const index = this.patientVisits.findIndex(visit => visit.visitId === updatedVisit.visitId);
      if (index !== -1) {
        this.patientVisits.splice(index, 1, updatedVisit);
      }
      this.showUpdateVisit = false; // Masquez le formulaire de mise à jour
    }
  }
};
</script>

<style scoped>

</style>
