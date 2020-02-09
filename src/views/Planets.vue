<template>
    <div>
        <h1>Planètes</h1>
        <div v-for="(planet, index) of planets" v-bind:key="index">
            <span v-on:click="onPlanetClicked(planet)"> {{ planet.name }} {{ index }} </span>
        </div>
        <br/>
        <PlanetDetails v-on:nameClicked="onNameClicked" v-if="selectedPlanet !== undefined" v-bind:planet="selectedPlanet"></PlanetDetails>
    </div>
</template>
<script>
import axios from 'axios'; 
import PlanetDetails from '@/components/PlanetDetails.vue';

export default {
    components: {
        PlanetDetails
    },
    name:'planets',
    created() {
        axios.get('https://swapi.co/api/planets').then(response => {
            this.planets = response.data.results;
        });
    },
    data: function() {
        return {
            planets: [],
            selectedPlanet: undefined
        }
    },
    methods: {
        onPlanetClicked(planet) {
            this.selectedPlanet = planet;
        },
        onNameClicked() {
            alert("Le nom a été cliqué");
        }
    }
}
</script>