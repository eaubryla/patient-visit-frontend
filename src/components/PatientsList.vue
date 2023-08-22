<template>
  <div>
    <h2>Choix du patient</h2>
    <div>
      <label>Patient : </label>
      <select class="col" v-model="selectedPatient" @change="onChange()">
        <option  v-for="patient in patients" :key="patient.patientId" :value="patient.patientId">{{ patient.name }} {{ patient.surname }} - {{ patient.dateOfBirth }} - {{ patient.socialSecurityNumber }}</option>
      </select>
    </div>
    <button @click="ConsultVisit">Visualiser les consultations</button>
    <button @click="CreateVisit">Créer une consultation</button>
    <ConsultVisit v-if="showConsultVisitForm" :selectedPatient="selectedPatient" />
    <CreateVisit v-if="showCreateVisitForm" :selectedPatient="selectedPatient" />
  </div>
</template>

<script>
import axios from "axios";
import ConsultVisit from "@/components/ConsultVisit.vue";
import CreateVisit from "@/components/CreateVisit.vue";

export default {
  data() {
    return {
      patients: [],
      selectedPatient: null,
      showConsultVisitForm: false,
      showCreateVisitForm: false
    };
  },
  components: {
    ConsultVisit,
    CreateVisit
  },
  mounted() {
    this.fetchPatients();
  },
  methods: {
    onChange() {
      this.showConsultVisitForm = false;
      this.showCreateVisitForm = false;
    },
    ConsultVisit() {
      this.showConsultVisitForm = true;
      this.showCreateVisitForm = false;
    },
    CreateVisit() {
      this.showCreateVisitForm = true;
      this.showConsultVisitForm = false;
    },
    fetchPatients() {
      axios.get("http://localhost:8080/api/patients") // URL du backend
          .then(response => {
            this.patients = response.data;
          })
          .catch(error => {
            console.error("Erreur lors de la récupération des patients :", error);
          });
    }
  }
};
</script>

<style scoped>

</style>
