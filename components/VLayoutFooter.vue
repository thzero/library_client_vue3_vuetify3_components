<template>
	<q-footer
		elevated
		class="bg-secondary text-white"
	>
		<q-toolbar>
			<VVersion :value="version" />
			<q-toolbar-title>
				© 2020
				<a
					class="ml-1"
					target="_blank"
					href="https://thzero.com"
				>
					thZero.com
				</a>
			</q-toolbar-title>
			<span
				v-if="isDev"
				style="padding-left: 4px;"
			>
				{{ breakpointName }}
			</span>
		</q-toolbar>
	</q-footer>
	<!-- <q-footer
		elevated
		class="bg-secondary text-white"
	>
		<VVersion v-model="version" />
		<q-space />
		© 2020
		<a
			class="ml-1"
			target="_blank"
			href="https://thzero.com"
		>
			thZero.com
		</a>
		<span
			v-if="isDev"
			style="padding-left: 4px;"
		>
			{{ breakpointName }}
		</span>
	</q-footer> -->
</template>

<script>
import { computed, getCurrentInstance, onMounted, ref } from 'vue';
import { useStore } from 'vuex';

import LibraryUtility from '@thzero/library_common/utility';

import base from './base';
import VVersion from './VVersion';

export default {
	name: 'LayoutFooter',
	components: {
		VVersion
	},
	extends: base,
	setup() {
		const version = ref({});

		const breakpointName = computed(() => {
			return LibraryUtility.isDev ? '// TODO' : '';
		});
		const isDev = computed(() => {
			return LibraryUtility.isDev;
		});

		const store = useStore();
		const instance = getCurrentInstance();

		onMounted(async () => {
			await store.dispatcher.root.getVersion(instance.ctx.correlationId());
			version.value = store.state.version;
		});

		return Object.assign(base.setup(), {
			version,
			breakpointName,
			isDev
		});
	}
	// data: () => ({
	// 	version: {}
	// }),
	// computed: {
	// 	breakpointName() {
	// 		// return LibraryUtility.isDev ? this.$vuetify.breakpoint.name : '';
	// 		return LibraryUtility.isDev ? '// TODO' : '';
	// 	},
	// 	isDev() {
	// 		return LibraryUtility.isDev;
	// 	}
	// },
	// async created() {
	// 	await this.$store.dispatcher.root.getVersion(this.correlationId());
	// 	this.version = this.$store.state.version;
	// }
};
</script>

<style scoped>
</style>
