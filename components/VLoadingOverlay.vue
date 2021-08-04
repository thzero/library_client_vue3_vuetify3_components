<template>
	<div>
	</div>
</template>

<script>
import { computed, onBeforeUnmount, watch } from 'vue';
import { useQuasar } from 'quasar';

import GlobalUtility from '@thzero/library_client/utility/global';

import baseLoadingOverlay from './baseLoadingOverlay';

export default {
	name: 'LoadingOverlay',
	extends: baseLoadingOverlay,
	setup (props) {
		const $q = useQuasar();
		let timer = null;

		const close = () => {
			if (timer !== null) {
				clearTimeout(timer);
				$q.loading.hide();
				timer = null;
			};
		};

		if (props.timeout) {
			onBeforeUnmount(() => close);
		}

		const displayMessage = computed(() => {
			return !String.isNullOrEmpty(props.message) ? props.message : GlobalUtility.$trans.t('messages.loading');
		});

		const signalI = computed(() => {
			return props.signal;
		});

		watch(signalI, (value, prevValue) => {
			if (value) {
				$q.loading.show({
					message: displayMessage
				});

				if (props.timeout) {
					timer = setTimeout(() => {
						$q.loading.hide();
						timer = null;
					}, props.timeout);
				}

				return;
			}

			close();
		});

		return Object.assign(baseLoadingOverlay.setup(props), {
			displayMessage
		});
	}
};
</script>

<style scoped>
</style>
