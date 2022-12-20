<template>
	<v-text-field
		v-model="innerValue"
		:readonly="readonly"
		v-bind="$attrs"
		@blur="blur"
		@update:modelValue="change"
	/>
</template>

<script>
import baseControlEdit from '@/library_vue/components/baseControlEdit';

export default {
	name: 'VtTextField',
	extends: baseControlEdit,
	props: {
		// must be included in props
		value: {
			type: null,
			default: null
		},
		blur: {
			type: Function,
			default: () => {}
		},
		readonly: {
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
