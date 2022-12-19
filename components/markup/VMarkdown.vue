<template>
	<div>
		<!-- eslint-disable vue/no-v-html -->
		<div
			class="markdown-body"
			v-html="display"
		/>
		<!--eslint-enable-->
	</div>
</template>

<script>
import baseMarkdown from '@/library_vue/components/markup/baseMarkdown';

export default {
	name: 'VtMarkdown',
	extends: baseMarkdown
};
</script>

<style scoped>
</style>
