<template>
	<ValidationProvider
		:vid="vid"
		:name="$attrs.label"
		:rules="rules"
		:bail="rulesBail"
		:immediate="true"
	>
		<v-switch
      		v-model="innerValue"
      		:label="$attrs.label"
			:disabled="disabled"
			@change="change"
    	></v-switch>
	</ValidationProvider>
</template>

<script>
import baseControlEdit from '@/library_vue/components/baseControlEdit';

export default {
	name: 'VtSwitchWithValidation',
	extends: baseControlEdit,
	props: {
		disabled: {
			type: Boolean,
			default: false
		},
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
			type: Boolean,
			default: false
		}
	},
	setup (props) {
		return Object.assign(baseControlEdit.setup(props), {
		});
	},
	watch: {
		// Handles external model changes.
		value(newVal) {
			this.initValue(newVal);
		}
	},
	mounted() {
		this.initValue(this.value);
	}
};
</script>

<style scoped>
</style>
