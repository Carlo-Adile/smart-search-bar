<script>
import Fuse from 'fuse.js';
import datasets from '../datasets/datasets.json';

export default {
	name: 'SearchBar',
	data() {
		return {
			datasets: datasets,
			selectedDataset: null,
			searchQuery: ''
		}
	},
	computed: {
		filteredResults() {
			const fuse = new Fuse(this.datasets[this.selectedDataset], {
				includeScore: true,
				threshold: 0.5,
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
			<div class="col-12 text-center mb-3">
				<h1>Smart SearchBar</h1>
				<p class="text-secondary fst-italic fw-medium">
					Semplifica la ricerca all'interno del tuo Sito Web grazie ad una barra di ricerca smart
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
				class="col-12 col-md-8 d-flex justify-content-bewteen align-items-center border rounded-pill p-1 shadow-sm mb-2 overflow-hidden">
				<div class="px-2 d-flex align-items-center w-100">
					<i class="fa-solid fa-magnifying-glass fs-5 ps-3 text-secondary"></i>
					<input type="text" autocomplete="off" placeholder=" 2 . Effettua una possibile ricerca"
						class="p-3 bg-light w-100" v-model="searchQuery">
				</div>
			</div>
			<!-- risultati del dataset -->
			<div class="col-12 col-md-8 mt-2 text-center" v-if="selectedDataset">
				<p class="mb-2 fw-medium">
					3 . Ottieni i risultati <span class="text-primary fw-bold">filtrati</span> dall'applicazione
				</p>
				<ul class="list-unstyled d-flex flex-wrap gap-2 justify-content-center">
					<li v-for="(item, index) in datasets[selectedDataset]" :key="index"
						class="border rounded rounded-pill shadow-sm px-lg-3 px-2 fs-6"
						:class="{ 'bg-primary text-white': filteredResults.includes(item) }">
						{{ item }}
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>