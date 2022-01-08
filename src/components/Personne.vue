<template>
  <div v-if="currentPersonne">
    <div class="form-group row">
      <label for="inputName" class="col-sm-2 col-form-label">Nom</label>
      <div class="col-sm-3">
        <input type="text" class="form-control" id="inputName" placeholder="Name" v-model="currentPersonne.name">
      </div>
    </div>
    <div class="form-group row">
      <label for="inputSurname" class="col-sm-2 col-form-label">Prénom</label>
      <div class="col-sm-3">
        <input type="text" class="form-control" id="inputSurname" placeholder="Surname" v-model="currentPersonne.surname">
      </div>
    </div>
    <div class="form-group row">
      <label for="inputPhone" class="col-sm-2 col-form-label">Téléphone</label>
      <div class="col-sm-3">
        <input type="text" class="form-control" id="inputPhone" placeholder="Phone" v-model="currentPersonne.phone">
      </div>
    </div>
    <div class="form-group row">
      <label for="inputCity" class="col-sm-2 col-form-label">Ville</label>
      <div class="col-sm-3">
        <input type="text" class="form-control" id="inputCity" placeholder="City" v-model="currentPersonne.city">
      </div>
    </div>
    <div class="form-group row">
      <div class="col-sm-3">
        <button class="badge badge-danger mr-2" @click="deletePersonne">Supprimer</button>
        <button type="submit" class="badge badge-success" @click="updatePersonne">Modifier</button> 
      </div>
    </div>
    <div v-if="update"><!-- Affichage du message diffèrent selon le retour de l'update initialisé dans le script -->
      <div class="alert alert-success">
        <p>{{ message }}</p> <!-- Update OK --> 
      </div>
    </div>
    <div v-else> <!-- Update NOK -->
        <div class="alert alert-light">
        <p>{{ message }}</p>
      </div>
    </div>
  </div>  
</template>

<script>
import PersonneDataService from "../services/PersonneDataService";

export default {
  name: "personne",
  data() {
    return {
      currentPersonne: null,
      update:false,
      message: ''
    };
  },
  methods: {
    getPersonne(id) {
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
            this.update=true;
          })
        .catch(e => {
            console.log(e);
          });
    },

    deletePersonne() {
      PersonneDataService.delete(this.currentPersonne.id)
        .then(response => {
            this.$router.push({ name: "personnes" });
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
