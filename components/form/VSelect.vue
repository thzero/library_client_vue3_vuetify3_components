<template>
	<v-select
		v-model="innerValue"
		:items="innerItems"
		:item-text="text"
		item-value="id"
		:hide-details="hideDetails"
		:label="$attrs.label"
		:flat="flat"
		:solo-inverted="soloInverted"
		@change="change"
	/>
</template>

<script>
import { useBaseControlEditComponent } from '@/library_vue/components/baseControlEdit';
import { useBaseControlEditProps } from '@/library_vue/components/baseControlEditProps';

export default {
	name: 'VtSelect',
	props: {
		...useBaseControlEditProps,
		flat: {
			type: Boolean,
			default: false
		},
		items: {
			type: [Object, Array],
			default: null
		},
		soloInverted: {
			type: Boolean,
			default: false
		}
	},
	setup (props, context) {
		const {
			correlationId,
			error,
			hasFailed,
			hasSucceeded,
			initialize,
			logger,
			noBreakingSpaces,
			notImplementedError,
			success,
			isSaving,
			serverErrors,
			setErrors,
			convertValue,
			errorI,
			errorsI,
			hideDetails,
			innerValue,
			initValue,
			innerValueUpdate
		} = useBaseControlEditComponent(props, context);
		
		const innerItems = ref([]);
		
		const text = (item) => { 
			return item.displayName ? item.displayName : item.name;
		}
		
		onMounted(async () => {
			if (props.items)
				innerItems.value = props.items;
			initValue(props.modelValue);
		});

		watch(() => props.items,
			(value) => {
				innerItems.value = value;
			}
		);

		return {
			correlationId,
			error,
			hasFailed,
			hasSucceeded,
			initialize,
			logger,
			noBreakingSpaces,
			notImplementedError,
			success,
			isSaving,
			serverErrors,
			setErrors,
			convertValue,
			errorI,
			errorsI,
			hideDetails,
			innerValue,
			initValue,
			innerValueUpdate,
			innerItems,
			text
		};
	},
	// data: () => ({
	// 	innerItems: []
	// }),
	// watch: {
	// 	// Handles external model changes.
	// 	items(newVal) {
	// 		this.innerItems = newVal;
	// 	},
	// 	// Handles external model changes.
	// 	value(newVal) {
	// 		this.initValue(newVal);
	// 	}
	// },
	// mounted() {
	// 	if (this.items)
	// 		this.innerItems = this.items;
	// 	this.initValue(this.value);
	// },
	// methods: {
	// 	text: (item) => item.displayName ? item.displayName : item.name
	// }
};
</script>

<style scoped>
</style>
