<template>
  <div v-if="!submitted">
    <div class="form-group row">
      <label for="inputName" class="col-sm-2 col-form-label">Nom</label>
      <div class="col-sm-3">
        <input type="text" class="form-control" id="inputName" placeholder="Nom" v-model="personne.name">
      </div>
    </div>
    <div class="form-group row">
      <label for="inputSurname" class="col-sm-2 col-form-label">Prénom</label>
      <div class="col-sm-3">
        <input type="text" class="form-control" id="inputSurname" placeholder="Prénom" v-model="personne.surname">
      </div>
    </div>
    <div class="form-group row">
      <label for="inputPhone" class="col-sm-2 col-form-label">Téléphone</label>
      <div class="col-sm-3">
        <input type="text" class="form-control" id="inputPhone" placeholder="Téléphone" v-model="personne.phone">
      </div>
    </div>
    <div class="form-group row">
      <label for="inputCity" class="col-sm-2 col-form-label">Ville</label>
      <div class="col-sm-3">
        <input type="text" class="form-control" id="inputCity" placeholder="Mettre DK :-)" v-model="personne.city">
      </div>
    </div>
    <div class="form-group row">
      <label for="inputSubmit" class="col-sm-2 col-form-label"></label>
      <div class="col-sm-3">
        <button @click="creerPersonne" class="btn btn-success">Ajouter</button>
      </div>
    </div>
  </div>
  <div v-else>
    <h4>Personne ajoutée avec succès!</h4>
      <button class="btn btn-success" @click="resetForm">Ajouter une nouvelle personne</button>
  </div>
</template>

<script>
import PersonneDataService from "../services/PersonneDataService";

export default {
  name: "add-personne",
  data() {
    return {
      personne: {
        id: null,
        name: "",
        surname: "",
        phone: "",
        city: ""
      },
      submitted: false
    };
  },
  methods: {
    creerPersonne() {
      var data = {
        //id: this.personne.id,
        name:this.personne.name,
        surname:this.personne.surname,
        phone:this.personne.phone,
        city:this.personne.city
        // A COMPLETER
      };

       PersonneDataService.create(data)
        .then(response => {
            this.submitted=true;
            console.log(response.data);
          })
        .catch(e => {
            console.log(e);
          });
    },
    
    resetForm() {
      this.submitted = false;
      this.personne = {};
    }
  }
};
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>
