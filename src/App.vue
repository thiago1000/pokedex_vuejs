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
					cols="2"
					v-for="pokemon in filtered_pokemons" 
					:key="pokemon.name">
					<v-card @click="toggleModal(getId(pokemon))">
						<v-container>
							<v-row class="mx-0 d-flex justify-center">
								<img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${getId(pokemon)}.png`"
								:alt="pokemon.name"
								class="pokemon-name" />
								<h2 class="text-center text-capitalize">{{pokemon.name}}</h2>
							</v-row>
						</v-container>
					</v-card>
				</v-col>
			</v-row>
		</v-container>
		
		<div class="text-center">
			<v-dialog v-model="showModal" width="800">
				<v-card v-if="selectedPokemon">
					<v-container>
						<v-row class="d-flex align-center">
							<v-col cols="4">
								<img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${selectedPokemon.id}.png`"
								:alt="selectedPokemon.name"
								class="pokemon-name">
							</v-col>
							<v-col cols="8">
								<h1 class="text-capitalize">{{ selectedPokemon.name }}</h1>
							</v-col>
						</v-row>
					</v-container>
				</v-card>
			</v-dialog>
  		</div>
	</v-app>
</template>

<script>

import axios from 'axios'

export default {
	name: 'App',
	
	data() {
		return {
			pokemons: [],
			search: "",
			showModal: false,
			selectedPokemon: null,
		}
	},

	mounted() {
		axios.get("https://pokeapi.co/api/v2/pokemon?limit=151").then((response) => {
			this.pokemons = response.data.results;
		})
	},

	methods: {
		getId(pokemon) {
			return pokemon.url.split("/")[6];
		},

		toggleModal(id) {
			axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`).then((response) => {
				console.log(response)
				this.selectedPokemon = response.data;
				this.showModal = !this.showModal;
			});
		},
	},
	computed: {
		filtered_pokemons() {
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