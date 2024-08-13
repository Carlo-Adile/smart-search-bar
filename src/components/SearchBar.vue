<script>
import Fuse from 'fuse.js';
import datasets from '../datasets/datasets.json';
import pokedex from '../datasets/pokedex';

export default {
	name: 'SearchBar',
	data() {
		return {
			datasets: { ...datasets, pokedex },
			selectedDataset: null,
			searchQuery: '',
			threshold: 0.3,
		}
	},
	computed: {
		filteredResults() {
			const fuse = new Fuse(this.datasets[this.selectedDataset], {
				includeScore: true,
				threshold: this.threshold,
			});
			const results = fuse.search(this.searchQuery);
			return results.map(result => result.item);
		}
	}
}
</script>

<template>
	<div class="container bg-light rounded-5 mx-auto">
		<div class="row py-5 px-2 justify-content-center">
			<!-- header -->
			<div class="col-12 col-md-8 text-center mb-3">
				<h1>Smart SearchBar</h1>
				<p class="text-secondary fst-italic fw-medium px-md-5">
					Semplifica la ricerca all'interno del tuo Sito Web grazie ad una barra di ricerca smart, progettata
					per correggere errori di battitura e proporre risultati in base ai caratteri digitati.
				</p>
			</div>
			<!-- scegli un dataset -->
			<div class="col-12 mb-2 text-center">
				<label for="dataset-select" class="form-label fw-medium">1 . Seleziona un dataset</label>
				<select id="dataset-select" class="d-block mx-auto p-2 mb-3 border-0 shadow-sm" style="width:250px"
					v-model="selectedDataset">
					<option v-for="(items, key) in datasets" :value="key">
						{{ key }}
					</option>
				</select>
			</div>
			<!-- search bar text input -->
			<div
				class="col-12 col-md-8 d-flex justify-content-bewteen align-items-center border rounded-pill p-1 shadow-sm mb-4 overflow-hidden">
				<div class="px-2 d-flex align-items-center w-100">
					<i class="fa-solid fa-magnifying-glass fs-5 ps-3 text-secondary"></i>
					<input type="text" autocomplete="off" placeholder=" 2 . Effettua una possibile ricerca"
						class="p-3 bg-light w-100" v-model="searchQuery">
				</div>
			</div>
			<!-- risultati del dataset -->
			<div class="col-12 col-md-8 mb-3 text-center">
				<p class="mb-3 fw-medium">
					3 . Ottieni i <span class="text-primary fw-bold">risultati</span> della ricerca
				</p>
				<ul v-if="selectedDataset && searchQuery.length >= 3"
					class="list-unstyled d-flex flex-wrap gap-2 justify-content-center">
					<li v-for="(item, index) in filteredResults" :key="index"
						class="border rounded rounded-pill shadow-sm px-lg-3 px-2 fs-6 bg-primary text-white">
						{{ item }}
					</li>
				</ul>
			</div>
			<!-- regola threshold -->
			<div class="col-12 col-md-6 mb-3 text-center">
				<label for="threshold-slider" class="form-label fw-medium">4 . Regola la tolleranza</label>
				<input type="range" id="threshold-slider" class="form-range" min="0.1" max="0.4" step="0.1" v-model="threshold">
				<p>valore attuale - {{ threshold }}</p>
			</div>
		</div>
	</div>
</template>