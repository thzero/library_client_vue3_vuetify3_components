<template>
	<v-footer
		app
		absolute
	>
		<VVersion v-model="version" />
		<v-spacer />
		<VCopyright v-model="version" />
		<span
			v-if="isDev"
			style="padding-left: 4px;"
		>
			{{ breakpointName }}
		</span>
	</v-footer>
</template>

<script>
import { computed, onMounted, ref } from 'vue';
import { useDisplay } from 'vuetify';

import LibraryClientConstants from '@thzero/library_client/constants';

import LibraryClientUtility from '@thzero/library_client/utility/index';
import LIbraryCommonUtility from '@thzero/library_common/utility';

import { useBaseComponent } from '@/library_vue/components/base';

import VCopyright from './VCopyright';
import VVersion from './VVersion';

export default {
	name: 'VtLayoutFooter',
	components: {
		VCopyright,
		VVersion
	},
	setup(props, context) {
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
		} = useBaseComponent(props, context);

		const useDisplayI = useDisplay();

		const serviceStore = LibraryClientUtility.$injector.getService(LibraryClientConstants.InjectorKeys.SERVICE_STORE);

		const version = ref({});

		const breakpointName = computed(() => {
			return LIbraryCommonUtility.isDev ? useDisplayI.name : '';
		});
		const isDev = computed(() => {
			return LIbraryCommonUtility.isDev;
		});

		onMounted(async () => {
			await serviceStore.dispatcher.requestVersion(correlationId());
			version.value = serviceStore.state.version;
		});

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
			breakpointName,
			isDev,
			version
		};
	}
};
</script>

<style scoped>
</style>
