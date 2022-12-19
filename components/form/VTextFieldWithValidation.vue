<template>
	<v-text-field
		v-model="innerValue"
		:error="errorI"
		:hide-details="hideDetails"
		:readonly="readonly"
		:disabled="disabled"
		:hint="$attrs.hint"
		:label="$attrs.label"
		:counter="maxcount"
		:density="density"
		@blur="blur"
	>
		<template v-slot:append>
			<span :class="countClass">{{ count }}</span>
		</template>
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
	name: 'VtTextFieldWithValidation',
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
		density: {
			type: String,
			default: 'compact'
		},
		disabled: {
			type: Boolean,
			default: false
		},
		hideDetails: {
			type: Boolean,
			default: false
		},
		maxcount: {
			type: Number,
			default: null
		},
		mincount: {
			type: Number,
			default: null
		},
		readonly: {
			type: Boolean,
			default: false
		},
		// must be included in props
		modelValue: {
			type: null,
			default: null
		}
	},
	computed: {
		count() {
			return this.maxcount ? '(' + (this.innerValue ? this.innerValue.length : 0) + ')' : '';
		},
		countClass() {
			return (this.maxcount && !String.isNullOrEmpty(this.innerValue) ? this.innerValue.length > this.maxcount ? 'negative ' : '' : '') + 'title-body2';
		},
		errorI() {
			return this.validation ? this.validation[this.vid] ? this.validation[this.vid].$error : false : true;
		},
		errorsI() {
			return this.validation ? this.validation[this.vid] ? this.validation[this.vid].$errors : [] : [];
		}
	}
};
</script>

<style scoped>
</style>
