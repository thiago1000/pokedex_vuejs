<template>
    <v-card @click="show_pokemon(get_id(pokemon))">
        <v-container>
            <v-row class="mx-0 d-flex justify-center">
                <img
                    :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${get_id(pokemon)}.png`"
                    :alt="pokemon.name"
                    class="pokemon-name"
                />
            </v-row>
            <h2 class="text-center text-capitalize">{{ pokemon.name }}</h2>
        </v-container>
    </v-card>
</template>

<script>
export default {
    name: "PokemonCard",

    props: {
        pokemon: Object,
        flat: {
            type: Boolean,
            default: false,
        },
    },
    methods: {
        get_id(pokemon) {
            return Number(pokemon.url.split("/")[6]);
        },
        show_pokemon(id) {
            console.log("clicou")
            this.$emit("clicked", id);
        },
    },
};
</script>

<style>
    .pokemon-name {
        width: 80%;
    }
</style>