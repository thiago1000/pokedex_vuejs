<template>
	<v-app>
		<v-container>
			<v-form>
				<v-container>
					<v-row>
						<v-col cols="12">
							<v-text-field
								v-model="search"
								label="Pesquisar"
							></v-text-field>
						</v-col>
					</v-row>
				</v-container>
			</v-form>
			<v-row>
				<v-col
					cols="6"
					md="2"
					v-for="pokemon in filteredPokemons" 
					:key="pokemon.name">
					<PokemonCard :pokemon="pokemon" @clicked="show_pokemon" />
				</v-col>
			</v-row>
		</v-container>
		
		<PokemonInfoDialog
			v-model:show="show_dialog"
			:selected_pokemon="selected_pokemon"
    	/>
	</v-app>
</template>

<script>

import axios from 'axios'
import PokemonCard from "./components/PokemonCard.vue";
import PokemonInfoDialog from "./components/PokemonInfoDialog.vue";

export default {
	name: 'App',

	components: {
		PokemonCard,
		PokemonInfoDialog,
  	},
	
	data() {
		return {
			pokemons: [],
			search: "",
			show_dialog: false,
			selected_pokemon: null,
		};
	},

	mounted() {
		axios.get("https://pokeapi.co/api/v2/pokemon?limit=251").then((response) => {
			this.pokemons = response.data.results;
		})
	},

	methods: {
		show_pokemon(id) {
			axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`).then((response) => {
				this.selected_pokemon = response.data;
				this.show_dialog = !this.show_dialog;
			});
		},

		getMoveLevel(move) {
			for (let version of move.version_group_details) {
				if ( version.version_group.name == "sword-shield" && version.move_learn_method.name == "level-up") {
					return version.level_learned_at;
				}
			}
			return 0;
		},
		
		// filterMoves(pokemon) {
		// 	return pokemon.moves.filter((item) => {
		// 		let include = false;
		// 		for (let version of item.version_group_details) {
		// 			// console.log(version.version_group);
		// 			if(version.version_group.name == "sword-shield" && version.move_learn_method.name == "level-up") {
		// 				include = true;
		// 			}
		// 		}
		// 		return include;
		// 	});
		// }
	},
	computed: {
		filteredPokemons() {
			return this.pokemons.filter((item)=>{
				return item.name.includes(this.search)
			})
		}
	}
}
</script>

<style>
#app {
	background: linear-gradient(
		to bottom right,
		rgba(10, 10, 10, 1),
		rgba(12, 39, 63, 1)
	) 
	no-repeat center center fixed !important;
	background-size: cover !important;
	background-position: center;
	min-height: 100vh;
}
.pokemon-name {
	width: 80%;
}
</style>