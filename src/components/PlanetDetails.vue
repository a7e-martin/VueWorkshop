<template>
    <div>
        <h1> {{ planet.name }} </h1>
        <div>
            <span>{{planet.climate}}</span><br/>
            <span v-show="planet.climate === 'frozen'">Attention, le climat est dangereux !</span>
            <p>{{description}}</p>
        </div>
    </div>
</template>
<script>
export default {
    name: 'planet-details',
    computed: {
        description: function() {
            return `La planète ${this.planet.name} est une planète de type ${this.planet.terrain}. Son climat est de type : ${this.planet.climate}`;
        }
    },
    props: [
        'planet'
    ]
}
</script>