<template>
	<div>
		<div class="headline text-center text-h6">
			{{ label }}
		</div>
		<div>
			<v-form>
				<slot />
				<div
					v-for="(item, index) in serverErrors"
					:key="index"
					class="red--text text--lighten-1 v-messages"
				>
					{{ item }}
				</div>
			</v-form>
		</div>
		<div
			v-if="!autoSave"
			class="text-right"
		>
			<v-spacer />
			<v-btn
				v-if="buttonDelete"
				color="primary lighten-1"
				text
				@click="handleDelete"
				class="mr-2"
			>
				{{ $t('buttons.delete') }}
			</v-btn>
			<v-btn
				v-if="buttonClear"
				color="primary lighten-1"
				text
				@click="handleClear"
				class="mr-2"
			>
				{{ $t('buttons.clear') }}
			</v-btn>
			<v-btn
				v-if="buttonOk"
				:disabled="(invalid || disabled)"
				color="green darken-1"
				text
				@click="submit"
			>
				{{ $t('buttons.ok') }}
			</v-btn>
		</div>
		<v-overlay
			absolute
			:value="overlaySave"
		>
			<v-card
				color="primary"
				dark
			>
				<v-card-text>
					Saving...
					<v-progress-linear
						indeterminate
						color="white"
						class="mb-0"
					></v-progress-linear>
				</v-card-text>
			</v-card>
		</v-overlay>
		<VConfirmationDialog
			v-if="buttonDelete"
			:non-recoverable="nonRecoverable"
			:signal="dialogDeleteConfirmSignal.signal"
			@cancel="dialogDeleteConfirmSignal.cancelI()"
			@ok="handleDeleteConfirmOk"
		/>
	</div>
</template>

<script>
import { computed, getCurrentInstance, onMounted, ref } from 'vue';

import LibraryConstants from '@thzero/library_client/constants';

import VueUtility from '@thzero/library_client_vue3/utility/index';

import VConfirmationDialog from '@/library_vue_vuetify/components/VConfirmationDialog';
import baseEdit from '@/library_vue/components/baseEdit';

import DialogSupport from '@/library_vue/components/support/dialog';

export default {
	name: 'VtFormControl',
	components: {
		VConfirmationDialog
	},
	extends: baseEdit,
	props: {
		autoSave: {
			type: Boolean,
			default: false
		},
		buttonCancel: {
			type: Boolean,
			default: true
		},
		buttonClear: {
			type: Boolean,
			default: true
		},
		buttonClearName: {
			type: String,
			default: 'buttons.clear'
		},
		buttonDelete: {
			type: Boolean,
			default: false
		},
		buttonOk: {
			type: Boolean,
			default: true
		},
		buttonOkName: {
			type: String,
			default: 'buttons.ok'
		},
		label: {
			type: String,
			default: ''
		},
		preCompleteDelete: {
			type: Function,
			default: null
		},
		preCompleteOk: {
			type: Function,
			default: null
		},
		resetForm: {
			type: Function,
			default: null
		},
		validation: {
			type: Object,
			default: null
		}
	},
	setup(props) {
		const instance = getCurrentInstance();

		return Object.assign(baseEdit.setup(props), {
		});
	},
	data: () => ({
		dialogLocationDeleteSignal: new DialogSupport(),
		disabled: false,
		internalItem: null,
		invalid: true,
        isSaving: false,
	}),
	watch: {
		// Handles external model changes.
		validation(value) {
			// console.log('v.invalid: ' + value.$invalid);
			// console.log('v.error: ' + value.$error);
			// console.log('v.errors: ' + JSON.stringify(value));
			this.invalid = value.$invalid;
			// console.log('v.invalid: ' + this.invalid);
		}
	},
	computed: {
		overlaySave() {
			return this.isSaving && this.autoSave;
		}
	},
	methods: {
		handleClear(correlationId) {
			this.logger.debug('FormControl', 'clear', 'clear', null, correlationId);
			// this.$nextTick(() => {
			// 	// this.$refs.obs.reset(correlationId);
			// });
			this.reset(this.correlationId(), false);
		},
		async handleDelete() {
			this.serverErrors = [];
			this.dialogDeleteConfirmSignal.open(this.correlationId());
		},
		async handleDeleteConfirmOk() {
			this.serverErrors = [];

			const correlationId = this.correlationId();

			this.dialogDeleteConfirmSignal.ok();

			if (this.preCompleteDelete) {
				const response = await this.preCompleteDelete(correlationId);
				this.logger.debug('FormDialog', 'handleDeleteConfirmOk', 'response', response, correlationId);
				if (this.hasFailed(response)) {
					// VueUtility.handleError(this.$refs.obs, this.serverErrors, response, correlationId);
					return;
				}
			}

			this.logger.debug('FormControl', 'handleDeleteConfirmOk', 'delete', null, correlationId);
			this.handleClear(correlationId);
		},
		async reset(correlationId, value) {
			await this.resetFormI(correlationId, value);
			this.logger.debug('FormControl', 'clear', null, null, correlationId);
			this.serverErrors = [];
			await this.validation.$validate();
			this.isSaving = false;
			// const timer = setInterval(async () => {
			// 	clearInterval(timer);
			// 	const el = document.getElementsByClassName('v-card__text');
			// 	if (el && el.length > 0)
			// 		el[0].scrollTop = 0;
			// }, 25);
		},
		async resetFormI(correlationId, value) {
			if (this.resetForm)
				this.resetForm(correlationId, value);
		},
		// eslint-disable-next-line
		resetControl(correlationId, value) {
		},
		setErrors(errors) {
			this.isSaving = false;
		},
		async submit() {
			this.isSaving = true;
			this.serverErrors = [];
			const correlationId = this.correlationId();

			const result = await this.validation.$validate();
			this.logger.debug('FormControl', 'submit', 'result', result, correlationId);
			if (!result)
				return;

			let response = { success: true, route: null };
			if (this.preCompleteOk) {
				response = await this.preCompleteOk(correlationId);
				this.logger.debug('FormControl', 'submit', 'response', response, correlationId);
				if (this.hasFailed(response)) {
					VueUtility.handleError(this.$refs.obs, this.serverErrors, response, correlationId);
					this.isSaving = false;
					return;
				}
			}

			this.logger.debug('FormControl', 'submit', 'ok', null, correlationId);
			this.$emit('ok');
			this.isSaving = false;
		}
	}
};
</script>

<style scoped>
</style>
