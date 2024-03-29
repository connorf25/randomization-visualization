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
			<b-card title="Study Information" class="m-5">
				<TheInitializationForm @submit="initializeParticipants" />
			</b-card>
		</div>
		<!-- Main layout -->
		<div v-if="info">
			<b-row>
				<b-col class="m-3">
					<BaseGroup
						@change="unassignedParticipantsLabel = $event"
						:label="unassignedParticipantsLabel"
						:participants="unassignedParticipants"
						:unit-of-randomization="info.unitOfRandomization"
						:number-confounding-factors="info.numberConfoundingFactors"
					/>
				</b-col>
			</b-row>
			<b-row>
				<b-col class="m-3">
					<h5>Assign Participant</h5>
					<b-button
						v-for="(group, index) in groups"
						:key="index"
						@click="assignParticipant(index)"
						class="mr-2"
						variant="primary"
					>
						{{ group.name }}
					</b-button>
					<b-button @click="assignParticipant(getRandomGroup())" class="mr-2" variant="info">Random</b-button>
					<b-button v-if="!isRandomizing" @click="assignAllParticipants()" variant="warning">Randomise All</b-button>
					<b-button v-if="isRandomizing" @click="isRandomizing = false" variant="danger">Stop</b-button>
				</b-col>
			</b-row>
			<b-row>
				<b-col class="m-3" v-for="(group, index) in groups" :key="index">
					<BaseGroup
						@change="updateName(index, $event)"
						:label="group.name"
						:participants="group.participants"
						:unit-of-randomization="info.unitOfRandomization"
						:number-confounding-factors="info.numberConfoundingFactors"
					/>
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
			unassignedParticipantsLabel: "Unassigned Participants",
			// Holds the participants assigned to groups
			groups: [],
			// Whether randomization is in progress
			isRandomizing: false,
		}
	},
	methods: {
		// Get random int between min and max inclusive
		getRandomInt(min, max) {
			min = Math.ceil(min);
			max = Math.floor(max);
			return Math.floor(Math.random() * (max - min + 1)) + min;
		},
		// Get a random confounding factor (participant color)
		getRandomColorGroup() {
			return this.getRandomInt(0, this.info.numberConfoundingFactors - 1)
		},
		// Get a random group index
		getRandomGroup() {
			return this.getRandomInt(0, this.info.numberGroups - 1)
		},
		// Initialize participants and groups depending on input
		initializeParticipants($event) {
			this.unassignedParticipants = [];
			this.groups = [];
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
					this.unassignedParticipants.push({ colorGroup: this.getRandomColorGroup() });
				}
			}
		},
		// Assign a participant to a particular group
		assignParticipant(groupIndex) {
			if (this.unassignedParticipants.length > 0) {
				const participant = this.unassignedParticipants.pop();
				this.groups[groupIndex].participants.push(participant);
			}
		},
		// Assign all participants with delay
		assignAllParticipants() {
			this.isRandomizing = true;
			setTimeout(() => {
				if(this.unassignedParticipants.length > 0 && this.isRandomizing) {
					this.assignParticipant(this.getRandomGroup());
					this.assignAllParticipants();
				} else {
					this.isRandomizing = false;
					return
				}
			}, 500)
		},
		// Update group name
		updateName(index, newName) {
			this.$set(this.groups[index], "name", newName);
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
