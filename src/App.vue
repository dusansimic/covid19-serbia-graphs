<template>
	<div id="app">
		<b-overlay id="overlay" :show="!loaded" spinner-variant="primary">
			<div v-if="loaded">
				<NavBar/>
				<SideBar :locationsList="locationsList" @area-picked="changeArea"/>

				<b-container>
					<b-row>
						<b-col>
							<Dashboard :data="data" :area-name="areaName"/>
						</b-col>
					</b-row>
				</b-container>
			</div>
		</b-overlay>
	</div>
</template>

<script>
import ky from 'ky';
import NavBar from './components/NavBar';
import SideBar from './components/SideBar';
import Dashboard from './components/Dashboard';

export default {
	name: 'App',
	components: {
		NavBar,
		SideBar,
		Dashboard
	},
	data() {
		return {
			loaded: false,
			areaName: null,
			locationsList: null,
			data: null
		};
	},
	mounted() {
		this.getData();
	},
	methods: {
		async getData() {
			this.data = await ky.get(`${process.env.VUE_APP_API_URL}`).json();
			this.locationsList = this.data.areaData.map(obj => obj.name);
			this.locationsList.unshift('Топ 5');
			this.loaded = true;
		},
		changeArea(area) {
			if (area === 'Топ 5') {
				this.areaName = null;
				return;
			}
			this.areaName = area;
			this.sidebarVisible = false;
		}
	}
};
</script>

<style scoped>
#app {
	min-height: 100vh;
}
#overlay {
	min-height: 100vh;
}
</style>
