<script>
import { computed } from 'vue';
import { useDisplay } from 'vuetify';

import LibraryClientVuetifyUtility from '@/library_vue_vuetify/utility/index';

import { useBaseEditComponent } from '@/library_vue/components/baseEdit';

export function useDisplayComponent(props, context, options) {
	const {
	} = useBaseEditComponent(props, context, options);

	const useDisplayI = useDisplay();

	const isFullscreen = computed(() => {
		return LibraryClientVuetifyUtility.fullscreen(useDisplayI);
	});

	return {
		isFullscreen
	};
};
</script>
