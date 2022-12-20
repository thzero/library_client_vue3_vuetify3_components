<template>
	<v-select
		v-model="innerValue"
		:error="errorI"
		:hide-details="hideDetails"
		:item-title="itemTitle"
		:item-value="itemValue"
		:items="innerItems"
		:label="$attrs.label"
		:multiple="multiple"
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
	</v-select>
</template>

<script>
import baseControlEdit from '@/library_vue/components/baseControlEdit';

export default {
	name: 'VtSelectWithValidation',
	extends: baseControlEdit,
	props: {
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
		items: {
			type: [Object, Array],
			default: null
		},
		itemTitle: {
			type: String,
			default: 'name'
		},
		itemValue: {
			type: String,
			default: 'id'
		},
		multiple: {
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
	data: () => ({
		innerItems: []
	}),
	watch: {
		// Handles external model changes.
		items(newVal) {
			this.innerItems = newVal;
		},
	},
	mounted() {
		if (this.items)
			this.innerItems = this.items;
		this.initValue(this.value);
	},
	methods: {
		text: (item) => item.displayName ? item.displayName : item.name,
	}
};
</script>

<style scoped>
</style>
