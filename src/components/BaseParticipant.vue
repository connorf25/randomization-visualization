<template>
	<div>
		<b-icon-building v-if="isCluster" class="h1" :variant="variants[group]"></b-icon-building>
		<b-icon-person-circle v-if="!isCluster" class="h1" :variant="variants[group]"></b-icon-person-circle>
	</div>
</template>

<script>
export default {
	name: 'HelloWorld',
	props: {
		group: Number,
		isCluster: Boolean
	},
	data() {
		return {
			variants: [
				"primary",
				"success",
				"warning",
				"danger",
				"info",
				"secondary",
				"dark"
			]
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
