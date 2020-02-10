<template>
    <div>
        <h1>Planètes</h1>
        <div>
            <label for="planet">Recherchez une planète : </label>
            <input type="text" name="planet" v-model="searchCriteria">
        </div>
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
    components: {
        PlanetDetails
    },
    name:'planets',
    created() {
        axios.get('https://swapi.co/api/planets').then(response => {
            this.planets = response.data.results;
        });
        this.debouncedSearch = debounce(this.searchPlanets, 400)
    },
    watch: {
        searchCriteria: function() {
            this.debouncedSearch();
        }
    },
    data: function() {
        return {
            planets: [],
            selectedPlanet: undefined,
            searchCriteria: ''
        }
    },
    methods: {
        onPlanetClicked(planet) {
            this.selectedPlanet = planet;
        },
        async searchPlanets() {
            const result = await axios.get(`https://swapi.co/api/planets/?search=${this.searchCriteria}`);
            this.planets = result.data.results;
        }
    }
}
</script>