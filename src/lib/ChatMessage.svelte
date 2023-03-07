<script lang="ts">
	import type { ChatCompletionRequestMessageRoleEnum } from 'openai';
	export let type: ChatCompletionRequestMessageRoleEnum;
	export let message: string;
</script>

<div class="cont">
	<div class="header">
		{type === 'user' ? 'Me' : 'GPT'}:
	</div>
	<div class="message">
		{message}
	</div>
</div>

<style lang="scss">
	.cont {
		display: flex;
		gap: 1rem;
		margin-bottom: 1rem;
	}
</style>
