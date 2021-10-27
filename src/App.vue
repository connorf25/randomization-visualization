<template>
	<div id="app">
		<!-- Navbar -->
		<b-navbar toggleable="lg" type="dark" variant="info">
			<b-navbar-brand href="#">Randomization Demonstration</b-navbar-brand>
			<b-navbar-nav class="ml-auto">
				<b-button @click="info = null" size="sm" variant="danger" class="my-2 my-sm-0">Close</b-button>
			</b-navbar-nav>
		</b-navbar>
		<!-- Initialization form -->
		<div v-if="!info">
			<b-card title="Initialization" class="m-5">
				<TheInitializationForm @submit="info = $event" />
			</b-card>
		</div>
		<!-- Main layout -->
		<div v-if="info">
			<BaseParticipant :colorGroup="0" :isCluster="false" />
		</div>
	</div>
</template>

<script>
import BaseParticipant from './components/BaseParticipant.vue'
import TheInitializationForm from './components/TheInitializationForm.vue'

export default {
	name: 'App',
	components: {
		BaseParticipant,
		TheInitializationForm
	},
	data() {
		return {
			info: null,
		}
	}
}
</script>

<style>
</style>
