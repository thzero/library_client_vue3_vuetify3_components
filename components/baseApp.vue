<script>
import GlobalUtility from '@thzero/library_client/utility/global';

import base from './base';

export default {
	name: 'BaseApp',
	extends: base,
	setup(props) {
		return Object.assign(base.setup(props), {
		});
	},
	async created() { // TODO: to setup() https://javascript.plainenglish.io/differences-between-vue-2-and-vue-3-ee627e2c83a8
		GlobalUtility.$EventBus.on('auth-refresh', async (user) => {
			const correlationId = this.correlationId();
			this.logger.debug('BaseApp', 'created', 'auth-refresh', user, correlationId);
			const items = await this.initialize(correlationId);
			if (!items)
				return;
			await Promise.all(items);
		});
	},
	async mounted() { // TODO: to setup() https://javascript.plainenglish.io/differences-between-vue-2-and-vue-3-ee627e2c83a8
		(async () => {
			const correlationId = this.correlationId();
			const items = await this.initialize(correlationId);
			if (!items)
				return;
			await Promise.all(items);
			// await Promise.all([
			// 	this.$store.dispatcher.root.initialize(correlationId),
			// 	this.$store.dispatcher.characters.initializeCharacters(correlationId)
			// ])
		})().catch(err => {
			// eslint-disable-next-line
			console.error(err);
		});
	},
	methods: { // TODO: to setup() https://javascript.plainenglish.io/differences-between-vue-2-and-vue-3-ee627e2c83a8????
		async initialize() {
			return null;
		}
	}
};
</script>

<style>
	.displayLink {
		cursor: pointer;
		text-decoration: underline;
	}
</style>
