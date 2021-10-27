<template>
	<div>
		<p>Participant</p>
		<p>{{color}}</p>
		<p>{{isCluster}}</p>
	</div>
</template>

<script>
export default {
	name: 'HelloWorld',
	props: {
		color: String,
		isCluster: Boolean
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
