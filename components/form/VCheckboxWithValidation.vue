<template>
	<ValidationProvider
		ref="prv"
		:vid="vid"
		:name="$attrs.label"
		:rules="rules"
		:bail="rulesBail"
		:immediate="true"
	>
		<v-checkbox
			v-model="innerValue"
			slot-scope="{ errors, valid }"
			:error-messages="errors"
			:success="valid"
			v-bind="$attrs"
			v-on="$listeners"
			@change="change"
		/>
	</ValidationProvider>
</template>

<script>
import baseControlEdit from '@/library_vue/components/baseControlEdit';

export default {
	name: 'VtCheckboxWithValidation',
	extends: baseControlEdit,
	props: {
		rules: {
			type: [Object, String],
			default: ''
		},
		rulesBail: {
			type: Boolean,
			default: true
		},
		rulesImmediate: {
			type: Boolean,
			default: false
		},
		change: {
			type: Function,
			default: () => {}
		},
		// must be included in props
		value: {
			type: null,
			default: null
		}
	},
	watch: {
		// Handles internal model changes.
		// innerValue(newVal) {
		//	 this.$emit('input', newVal)
		// },
		// Handles external model changes.
		value(newVal) {
			this.initValue(newVal);
		}
	},
	mounted() {
		this.initValue(this.value);
	},
	methods: {
		validation() {
			return this.$refs.prv;
		}
	}
};
</script>

<style scoped>
</style>
