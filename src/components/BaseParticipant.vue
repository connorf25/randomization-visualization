<template>
	<div>
		<p>Participant</p>
		<p>{{colorGroup}}</p>
		<p>{{colors[colorGroup]}}</p>
		<p>{{isCluster}}</p>
	</div>
</template>

<script>
export default {
	name: 'HelloWorld',
	props: {
		colorGroup: Number,
		isCluster: Boolean
	},
	data() {
		return {
			colors: [
				"#ed254e",
				"#f9dc5c",
				"#011936",
				"#465362"
			]
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
