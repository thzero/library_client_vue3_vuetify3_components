<script>
import { computed } from 'vue';
import { useDisplay } from 'vuetify';

import VueUtility from '@/library_vue_vuetify/utility/index';

import { useBaseEditComponent } from '@/library_vue/components/baseEdit';

export function useDisplayComponent(props, context, initializeI) {
	const {
	} = useBaseEditComponent(props, context, initializeI);

	const useDisplayI = useDisplay();

	const isFullscreen = computed(() => {
		return VueUtility.fullscreenDisplay(useDisplayI);
	});

	return {
		isFullscreen
	};
};
</script>
