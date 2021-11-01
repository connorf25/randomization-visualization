<template>
	<div>
		<b-form @submit="onSubmit" @reset="onReset" v-if="show">
			<b-form-group label="Number of treatment groups:">
				<b-form-input
					v-model="form.numberGroups"
					type="number"
					required
				></b-form-input>
			</b-form-group>

			<b-form-group label="Number of participants in each group:">
				<b-form-input
					v-model="form.numberParticipants"
					type="number"
					required
				></b-form-input>
			</b-form-group>

			<b-form-group label="Randomization type:">
				<b-form-select
					v-model="form.randomizationType"
					:options="randomizationTypeOptions"
					required
				></b-form-select>
			</b-form-group>

			<b-form-group label="Unit of randomization:">
				<b-form-select
					v-model="form.unitOfRandomization"
					:options="unitOfRandomizationOptions"
					required
				></b-form-select>
			</b-form-group>

			<b-form-group label="Number of confounding factors:">
				<b-form-input
					v-model="form.numberConfoundingFactors"
					type="number"
					required
				></b-form-input>
			</b-form-group>

			<b-button type="submit" class="mr-2" variant="primary">Initialize</b-button>
			<b-button type="reset" variant="danger">Reset</b-button>
		</b-form>
	</div>
</template>

<script>
export default {
	data() {
		return {
			show: true,
			form: {
				numberGroups: 2,
				numberParticipants: 20,
				randomizationType: "simple",
				unitOfRandomization: "individual",
				numberConfoundingFactors: 2,
			},
			randomizationTypeOptions: [
				{ text: 'Simple', value: 'simple' },
				{ text: 'Block', value: 'block', disabled: true },
				{ text: 'Stratified', value: 'stratified', disabled: true },
			],
			unitOfRandomizationOptions: [
				{ text: 'Individual', value: 'individual' },
				{ text: 'Cluster', value: 'cluster' },
			]
		}
	},
	methods: {
		resetForm() {
			this.form.numberGroups = 2;
			this.form.numberParticipants= 20;
			this.form.randomizationType= "simple";
			this.form.unitOfRandomization= "individual";
			this.form.numberConfoundingFactors= 2;
		},
		onSubmit(event) {
			event.preventDefault();
			this.$emit("submit", this.form);
		},
		onReset(event) {
			event.preventDefault()
			// Reset our form values
			this.resetForm();
			// Trick to reset/clear native browser form validation state
			this.show = false;
			this.$nextTick(() => {
				this.show = true;
			})
		}
	}
}
</script>
