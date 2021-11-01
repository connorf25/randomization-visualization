<template>
	<div id="app">
		<!-- Navbar -->
		<b-navbar toggleable="lg" type="dark" variant="info">
			<b-navbar-brand href="#">Randomisation Demonstration</b-navbar-brand>
			<b-navbar-nav class="ml-auto">
				<b-button @click="info = null" size="sm" variant="danger" class="my-2 my-sm-0">Close</b-button>
			</b-navbar-nav>
		</b-navbar>
		<!-- Initialization form -->
		<div v-if="!info">
			<b-card title="Initialization" class="m-5">
				<TheInitializationForm @submit="initializeParticipants" />
			</b-card>
		</div>
		<!-- Main layout -->
		<div v-if="info">
			<b-row>
				<b-col>
					<BaseGroup label="Unassigned Participants" :participants="unassignedParticipants" />
				</b-col>
			</b-row>
			<b-row>
				<b-col v-for="(group, index) in groups" :key="index">
					<BaseGroup :label="group.name" :participants="group.participants" />
				</b-col>
			</b-row>
		</div>
	</div>
</template>

<script>
import BaseGroup from './components/BaseGroup.vue'
import TheInitializationForm from './components/TheInitializationForm.vue'

export default {
	name: 'App',
	components: {
		BaseGroup,
		TheInitializationForm
	},
	data() {
		return {
			// Holds the info collected from the form
			info: null,
			// Holds all the unassigned participants
			unassignedParticipants: [],
			// Holds the participants assigned to groups
			groups: [],
		}
	},
	methods: {
		initializeParticipants($event) {
			this.info = $event;
			// Initialize groups
			for(let i = 0; i < this.info.numberGroups; i++) {
				switch (i) {
					case 0:
						this.groups.push({ name: "Treatment", participants: [] });
						break;
					case 1:
						this.groups.push({ name: "Control", participants: [] });
						break;
					default:
						this.groups.push({ name: "New Group", participants: [] });
						break;
				}
				// Initialize participants
				for(let j = 0; j < this.info.numberParticipants; j++) {
					this.unassignedParticipants.push({ colorGroup: 0 });
				}
			}
		}
	}
}
</script>

<style>
#app {
	min-height: 100vh;
	background-color: rgb(240, 240, 240);
}
.row {
	margin: 0px !important;
}
</style>
