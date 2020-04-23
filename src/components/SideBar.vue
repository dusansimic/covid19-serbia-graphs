<template>
	<b-sidebar id="sidebar-1" title="Areas" shadow backdrop>
		<div class="px-3 py-2">
			<b-form-input id="search-value-input" v-model="searchValue" placeholder="Search" @input="searchAreas()"></b-form-input>
			<b-list-group>
				<b-list-group-item v-for="location in shownLocations" :key="location" action @click="logSomething" class="noselect">{{ location }}</b-list-group-item>
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
				this.shownLocations = this.locationsList.filter(location => this.transliterate(location).toLowerCase().includes(this.transliterate(this.searchValue).toLowerCase()));
			} else {
				this.shownLocations = this.locationsList;
			}
		}, 500),
		logSomething(area) {
			this.$emit('area-picked', area.target.innerText)
		},
		transliterate(word) {
			const charMap = {
				'А': 'A', 'а': 'a', 'Б': 'B', 'б': 'b', 'В': 'V',
				'в': 'v', 'Г': 'G', 'г': 'g', 'Д': 'D', 'д': 'd',
				'Ђ': 'Đ', 'ђ': 'đ', 'Е': 'E', 'е': 'e', 'Ж': 'Ž',
				'ж': 'ž', 'З': 'Z', 'з': 'z', 'И': 'I', 'и': 'i',
				'Ј': 'J', 'ј': 'j', 'К': 'K', 'к': 'k', 'Л': 'L',
				'л': 'l', 'Љ': 'Lj', 'љ': 'lj', 'М': 'M', 'м': 'm',
				'Н': 'N', 'н': 'n', 'Њ': 'Nj', 'њ': 'nj', 'О': 'O',
				'о': 'o', 'П': 'P', 'п': 'p', 'Р': 'R', 'р': 'r',
				'С': 'S', 'с': 's', 'Т': 'T', 'т': 't', 'Ћ': 'Ć',
				'ћ': 'ć', 'У': 'U', 'у': 'u', 'Ф': 'F', 'ф': 'f',
				'Х': 'H', 'х': 'h', 'Ц': 'C', 'ц': 'c', 'Ч': 'Č',
				'ч': 'č', 'Џ': 'Dž', 'џ': 'dž', 'Ш': 'Š', 'ш': 'š'
			};

			return word.split('').map(char => {
				return charMap[char] || char;
			}).join('');
		}
	}
};
</script>

<style scoped>
#search-value-input {
	margin-bottom: 10px;
}
.noselect {
	-webkit-touch-callout: none; /* iOS Safari */
	-webkit-user-select: none; /* Safari */
	-khtml-user-select: none; /* Konqueror HTML */
	-moz-user-select: none; /* Old versions of Firefox */
	-ms-user-select: none; /* Internet Explorer/Edge */
	user-select: none; /* Non-prefixed version, currently supported by Chrome,
	Opera and Firefox */
}
</style>
