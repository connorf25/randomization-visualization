<template>
	<div>
		<b-card header-bg-variant="info" header-text-variant="white">
			<!-- Header -->
			<template #header>
				<div v-if="!isEdit">
					{{label}}
					<b-button @click="edit()" class="ml-2" variant="outline-light" size="sm">
						<b-icon-pencil-fill></b-icon-pencil-fill>
					</b-button>
				</div>
				<div v-if="isEdit" class="d-inline">
					<b-form-input v-model="newLabel" :autofocus="true" class="d-inline w-50 mr-2" size="sm"></b-form-input>
					<b-button @click="save(newLabel)" class="mr-2" variant="primary" size="sm">Save</b-button>
					<b-button @click="cancel()" variant="danger" size="sm">Cancel</b-button>
				</div>
			</template>
			<!-- Content -->
			<BaseParticipant
				class="d-inline m-1"
				v-for="(participant, index) in participants"
				:group="participant.colorGroup"
				:isCluster="unitOfRandomization == 'cluster'"
				:key="index"
			/>
			<!-- Footer -->
			<template #footer>
				{{ generateFooterText() }}
			</template>
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
		unitOfRandomization: String,
		numberConfoundingFactors: Number,
	},
	data() {
		return {
			isEdit: false,
			newLabel: "",
		}
	},
	methods: {
		edit() {
			this.newLabel = this.label;
			this.isEdit = true;
		},
		save(newLabel) {
			console.log(newLabel);
			this.$emit('change', newLabel);
			this.$emit('input', newLabel);
			this.isEdit = false;
		},
		cancel() {
			this.newLabel = this.label;
			this.isEdit = false;
		},
		countFactor(index) {
			return this.participants.filter(participant => participant.colorGroup == index).length;
		},
		generateFooterText() {
			var text = `Total: ${this.participants.length}`;
			var factors = [];
			if (this.participants.length > 0) {
				for(let i = 0; i < this.numberConfoundingFactors; i++) {
					factors.push(this.countFactor(i));
				}
				if (factors.length > 0) {
					text += ` [${factors.join(", ")}]`
				}
			}
			return text;
		}
	}
}
</script>