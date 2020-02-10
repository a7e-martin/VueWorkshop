<template>
    <div>
        <h1>Planètes</h1>
        <div>
            <label for="planet">Recherchez une planète : </label>
            <input type="text" name="planet" v-model="searchCriteria">
        </div>
        <!-- v-bind:key est obligatoire dans les boucles de rendu v-for -->
        <div v-for="(planet, index) of planets" v-bind:key="index">
            <span v-on:click="onPlanetClicked(planet)"> {{ planet.name }} {{ index }} </span>
        </div>
        <br/>
        <PlanetDetails v-if="selectedPlanet !== undefined" v-bind:planet="selectedPlanet"></PlanetDetails>
    </div>
</template>
<script>
import axios from 'axios'; 
import PlanetDetails from '@/components/PlanetDetails.vue';
import debounce from 'debounce';

export default {
    name:'planets',
    components: {
        PlanetDetails
    },
    /**
     * Created est appelée lorsque le composant est créé dans le DOM
     * Plus d'infos : https://vuejs.org/v2/guide/instance.html#Instance-Lifecycle-Hooks
     */
    created() {
        // 'debounce' permet de limiter l'appel à une fonction lorsque celle-ci est executée 'trop souvent'
        this.debouncedSearch = debounce(this.searchPlanets, 400)
    },
    /**
     * La section watch permet de définir des 'surveillants' de variables.
     * Lorsque la variable 'surveillée' est modifiée, la fonction correspondante va être executée.
     */
    watch: {
        searchCriteria: function() {
            this.debouncedSearch();
        }
    },
    /**
     * La section data recense toute les données (variables) utilisables par le composant
     */
    data: function() {
        return {
            planets: [],
            selectedPlanet: undefined,
            searchCriteria: ''
        }
    },
    /**
     * La section methods rassemble toutes les fonction (méthodes) utilisables par le composant
     */
    methods: {
        onPlanetClicked(planet) {
            this.selectedPlanet = planet;
        },
        /**
         * Récupère la liste des planètes depuis l'API
         */
        getPlanets() {
            axios.get('https://swapi.co/api/planets').then(response => {
                this.planets = response.data.results;
            });
        },
        /**
         * Récupère la liste des planètes correspondantes au critère de recherche @var searchCriteria
         * Cette méthode utilise la version async / await, en comparaison avec @see getPlanets()
         */
        async searchPlanets() {
            const result = await axios.get(`https://swapi.co/api/planets/?search=${this.searchCriteria}`);
            this.planets = result.data.results;
        }
    }
}
</script>