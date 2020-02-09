<template>
    <div>
        <h1 v-if="planet.name === undefined">Aucune planète sélectionnée</h1>
        <h1 v-else v-on:click="onClick"> {{ planet.name }} </h1>
    </div>
</template>
<script>
export default {
    name: 'planet-details',
    props: [
        'planet'
    ],
    methods : {
        onClick() {
            this.$emit('nameClicked')
        }
    }
}
</script>