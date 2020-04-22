<template>
	<b-sidebar id="sidebar-1" title="Areas" shadow backdrop>
		<div class="px-3 py-2">
			<b-form-input id="search-value-input" v-model="searchValue" placeholder="Search" @input="searchAreas()"></b-form-input>
			<b-list-group>
				<b-list-group-item v-for="location in shownLocations" :key="location" action @click="logSomething">{{ location }}</b-list-group-item>
			</b-list-group>
		</div>
	</b-sidebar>
</template>

<script>
import debounce from 'lodash/debounce';

export default {
	name: 'SideBar',
	props: {
		locationsList: Array
	},
	data() {
		return {
			shownLocations: null,
			searchValue: ''
		};
	},
	mounted() {
		this.shownLocations = this.locationsList;
	},
	methods: {
		searchAreas: debounce(function () {
			if (this.searchValue) {
				this.shownLocations = this.locationsList.filter(location => location.toLowerCase().includes(this.searchValue.toLowerCase()));
			} else {
				this.shownLocations = this.locationsList;
			}
		}, 500),
		logSomething(area) {
			this.$emit('area-picked', area.target.innerText)
		}
	}
};
</script>

<style scoped>
#search-value-input {
	margin-bottom: 10px;
}
</style>
