<template>
	<div id="app">
		<BaseParticipant color="#32a852" :isCluster="false" />
	</div>
</template>

<script>
import BaseParticipant from './components/BaseParticipant.vue'

export default {
	name: 'App',
	components: {
		BaseParticipant
	}
}
</script>

<style>
</style>
