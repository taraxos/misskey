<template>
<XWindow ref="window" :initial-width="400" :initial-height="500" :can-resize="true" @closed="$emit('closed')">
	<template #header>
		<i class="fas fa-exclamation-circle" style="margin-right: 0.5em;"></i>
		<I18n :src="$ts.reportAbuseOf" tag="span">
			<template #name>
				<b><MkAcct :user="user"/></b>
			</template>
		</I18n>
	</template>
	<div class="dpvffvvy _monolithic_">
		<div class="_section">
			<MkTextarea v-model="comment">
				<template #label>{{ $ts.details }}</template>
				<template #caption>{{ $ts.fillAbuseReportDescription }}</template>
			</MkTextarea>
		</div>
		<div class="_section">
			<MkButton @click="send" primary full :disabled="comment.length === 0">{{ $ts.send }}</MkButton>
		</div>
	</div>
</XWindow>
</template>

<script lang="ts">
import { defineComponent, markRaw } from 'vue';
import XWindow from '@client/components/ui/window.vue';
import MkTextarea from '@client/components/ui/textarea.vue';
import MkButton from '@client/components/ui/button.vue';
import * as os from '@client/os';

export default defineComponent({
	components: {
		XWindow,
		MkTextarea,
		MkButton,
	},

	props: {
		user: {
			type: Object,
			required: true,
		},
		initialComment: {
			type: String,
			required: false,
		},
	},

	emits: ['closed'],

	data() {
		return {
			comment: this.initialComment || '',
		};
	},

	methods: {
		send() {
			os.apiWithDialog('users/report-abuse', {
				userId: this.user.id,
				comment: this.comment,
			}, undefined, res => {
				os.dialog({
					type: 'success',
					text: this.$ts.abuseReported
				});
				this.$refs.window.close();
			});
		}
	},
});
</script>

<style lang="scss" scoped>
.dpvffvvy {
	--root-margin: 16px;
}
</style>
