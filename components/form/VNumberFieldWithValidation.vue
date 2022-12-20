<template>
	<v-text-field
		v-model="innerValue"
		type="number"
		:error="errorI"
		:hide-details="hideDetails"
		:readonly="readonly"
		:disabled="disabled"
		:hint="$attrs.hint"
		:label="$attrs.label"
		:class="displayClass"
		:density="density"
		v-bind="$attrs"
		@blur="blur"
	>
		<template v-slot:details>
			<div
				v-for="error of errorsI"
				:key="error.$uid"
			>
				<strong>{{ error.$message }}</strong>
				<small> on </small>
				<strong>{{ error.$property }}</strong>
			</div>
		</template>
	</v-text-field>
</template>

<script>
import baseControlEdit from '@/library_vue/components/baseControlEdit';

export default {
	name: 'VtNumberFieldWithValidation',
	extends: baseControlEdit,
	props: {
		blur: {
			type: Function,
			default: () => {}
		},
		change: {
			type: Function,
			default: () => {}
		},
		disabled: {
			type: Boolean,
			default: false
		},
		hideDetails: {
			type: Boolean,
			default: false
		},
		negativeColor: {
			type: Boolean,
			default: false
		},
		readonly: {
			type: Boolean,
			default: false
		},
	},
	setup (props) {
		return Object.assign(baseControlEdit.setup(props), {
		});
	},
	computed: {
		displayClass() {
			return this.negativeColor ? (this.value < 0 ? 'text-negative' : null) : null;
		},
	},
};
</script>

<style scoped>
</style>
