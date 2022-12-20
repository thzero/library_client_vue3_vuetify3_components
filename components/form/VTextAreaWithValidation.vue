<template>
	<v-textarea
		v-model="innerValue"
		:error-messages="errors"
		:hide-details="hideDetails"
		v-bind="$attrs"
		auto-grow
		clearable
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
	</v-textarea>
</template>

<script>
import baseControlEdit from '@/library_vue/components/baseControlEdit';

export default {
	name: 'VtTextAreaWithValidation',
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
		hideDetails: {
			type: Boolean,
			default: false
		}
	},
	setup (props) {
		return Object.assign(baseControlEdit.setup(props), {
		});
	},
	computed: {
		count() {
			return this.maxcount ? '(' + (this.innerValue ? this.innerValue.length : 0) + ')' : '';
		},
		countClass() {
			return (this.maxcount && !String.isNullOrEmpty(this.innerValue) ? this.innerValue.length > this.maxcount ? 'negative ' : '' : '') + 'title-body2';
		}
	}
};
</script>

<style scoped>
</style>
