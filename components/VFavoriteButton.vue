<template>
	<v-btn
		:disabled="disabled"
		depressed
		large
		style="min-width: 0px"
		@click="click(!innerValue)"
	>
		<v-icon
			v-if="!innerValue"
			color="blue"
		>
			mdi-star-outline
		</v-icon>
		<v-icon
			v-if="innerValue"
			color="blue"
		>
			mdi-star
		</v-icon>
	</v-btn>
</template>

<script>
import LIbraryCommonUtility from '@thzero/library_common/utility';

import baseControlEdit from '@/library_vue/components/baseControlEdit';

export default {
	name: 'VtFavoriteButton',
	extends: baseControlEdit,
	props: {
		// must be included in props
		value: {
			type: null,
			default: null
		},
		disabled: {
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
	},
	methods: {
		click(value) {
			this.update(this, value);
		},
		update: LIbraryCommonUtility.debounce(async function(self, value) {
			self.innerValue = value;
		}, 500)
	}
};
</script>

<style scoped>
</style>
