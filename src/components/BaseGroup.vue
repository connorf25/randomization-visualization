<template>
	<div class="p-3">
		<b-card :header="label">
			<BaseParticipant
				class="d-inline m-1"
				v-for="(participant, index) in participants"
				:group="participant.colorGroup"
				:isCluster="false"
				:key="index"
			/>
		</b-card>
	</div>
</template>

<script>
import BaseParticipant from './BaseParticipant.vue';

export default {
	name: "BaseGroup",
	components: {
		BaseParticipant
	},
	props: {
		label: String,
		participants: Array,
	}
}
</script>