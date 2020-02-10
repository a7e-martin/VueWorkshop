<template>
    <div>
        <h1 v-if="planet.name === undefined">Aucune planète sélectionnée</h1>
        <h1 v-else> {{ planet.name }} </h1>
    </div>
</template>
<script>
export default {
    name: 'planet-details',
    props: [
        'planet'
    ]
}
</script>