<template>
  <div class="container">
    <div class="row">
      <!-- Affichage en 2 colonnes, Colonne Gauche toutes les personnes, Colonne Droite la séléction -->
      <div class="col-sm"> <!--Colonne Gauche -->
        <ul class="list-group">
          <li class="list-group-item"
          :class="{ active: id == currentIndex }"
          v-for="(personne, id) in personnes"
          :key="id"
          @click="setActivePersonne(personne, id)"
          >
          {{ personne.surname }} {{ personne.name }}
          </li>
        </ul>
      </div>
    <div class="col-sm"> <!--Colonne Droite -->
      <div v-if="currentPersonne">
        <form>
          <fieldset disabled> <!--Ajout de la propriété disabled afin de ne pas modifier sur cette form -->
            <div class="form-group row">
              <label for="inputName" class="col-sm-2 col-form-label">Nom</label>
              <div class="col-sm-10">
                <input type="text" class="form-control" id="inputName" placeholder="Name" v-model="currentPersonne.name">
              </div>
            </div>
            <div class="form-group row">
              <label for="inputSurname" class="col-sm-2 col-form-label">Prénom</label>
              <div class="col-sm-10">
                <input type="text" class="form-control" id="inputSurname" placeholder="Surname" v-model="currentPersonne.surname">
              </div>
            </div>
            <div class="form-group row">
              <label for="inputPhone" class="col-sm-2 col-form-label">Téléphone</label>
              <div class="col-sm-10">
                <input type="text" class="form-control" id="inputPhone" placeholder="Phone" v-model="currentPersonne.phone">
              </div>
            </div>
            <div class="form-group row">
              <label for="inputCity" class="col-sm-2 col-form-label">Ville</label>
              <div class="col-sm-10">
                <input type="text" class="form-control" id="inputCity" placeholder="City" v-model="currentPersonne.city">
              </div>
            </div>
          </fieldset> <!--Fin de la propriété disabled afin de ne pas griser le bouton modifier -->
            <div class="form-group row">
              <label for="inputSubmit" class="col-sm-2 col-form-label"></label>
              <div class="col-sm-10">
                <router-link :to="'/personnes/' + currentPersonne.id"><button type="submit" class="btn btn-primary">Modifier</button></router-link>
              </div>
            </div>
          </form>
        </div>
        <div v-else>
          <p>Cliquez sur une des personnes pour afficher les détails.</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PersonneDataService from "../services/PersonneDataService";

export default {
  name: "personnes",
  data() {
    return {
      personnes: [],
      currentPersonne: null,
      currentIndex: -1,
    };
  },
  methods: {
    getPersonnes() {
      PersonneDataService.getAll()
        .then(response => {
          this.personnes = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },

    setActivePersonne(personne, index) {
      this.currentPersonne = personne;
      this.currentIndex = personne ? index : -1;
    },
  },
  mounted() {
    this.getPersonnes();
  }
};
</script>
