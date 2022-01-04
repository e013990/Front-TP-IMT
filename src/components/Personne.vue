<template>
  <!-- A COMPLETER -->
  <div v-if="currentPersonne">
    <input type="text" id="id" v-model="currentPersonne.id"><br>
    <input type="text" id="name" v-model="currentPersonne.name"><br>
    <input type="text" id="surname" v-model="currentPersonne.surname"><br>
    <input type="text" id="phone" v-model="currentPersonne.phone"><br>
    <input type="text" id="city" v-model="currentPersonne.city"><br>
    <!-- A INCLURE DANS LE FORM -->
      <button class="badge badge-danger mr-2"
        @click="deletePersonne"
      >
        Supprimer
      </button>

      <!-- A INCLURE DANS LE FORM -->
      <button type="submit" class="badge badge-success"
       @click="updatePersonne"
      >
        Modifier
      </button>
      <p>{{ message }}</p>
  </div>
</template>

<script>
import PersonneDataService from "../services/PersonneDataService";

export default {
  name: "personne",
  data() {
    return {
      currentPersonne: null,
      message: ''
    };
  },
  methods: {
    getPersonne(id) {
      // A COMPLETER
      PersonneDataService.get(id)
        .then(response => {
            this.currentPersonne = response.data;
            console.log(response.data);
          })
        .catch(e => {
            console.log(e);
          });
      },

    updatePersonne() {
      PersonneDataService.update(this.currentPersonne)
        .then(response => {
            this.message = 'Personne modifiée avec succès! ';
            console.log(response);
          })
        .catch(e => {
            console.log(e);
          });
    },

    deletePersonne() {
      PersonneDataService.delete(this.currentPersonne.id)
        .then(response => {
            this.$router.push({ name: "ROUTE_VERS_LISTE_PERSONNES" });
            console.log(response);
          })
        .catch(e => {
            console.log(e);
          });
    }
  },
  mounted() {
    this.message = '';
    this.getPersonne(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>
