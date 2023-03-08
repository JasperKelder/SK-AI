<script lang="ts">
	import { enhance, type SubmitFunction } from '$app/forms';
	import type { ActionData } from './$types';
	import { afterUpdate } from 'svelte';
	import { fly } from 'svelte/transition';

	export let form: ActionData;

	let loading = false;

	const onSubmit: SubmitFunction = () => {
		// before response
		loading = true;
		// after response
		return async ({ update }) => {
			loading = false;
			// run default update
			update();
		};
	};

	afterUpdate(() => {
		window.scrollTo(0, document.body.scrollHeight);
	});
</script>

<section class="head">
	<h1>Progressively Enhanced</h1>
	{#if form?.dialog.length}
		<form method="POST" action="?/clear" use:enhance>
			<button> X </button>
		</form>
	{/if}
</section>

<div>
	{#if form?.dialog}
		<section>
			{#each form?.dialog as { role, content }}
				<div>
					<p in:fly={{ duration: 400, y: 300 }}>
						{content.trim()}
					</p>
				</div>
			{/each}
		</section>
	{/if}

	<section>
		<form method="POST" action="?/chat" use:enhance={onSubmit}>
			<input type="hidden" name="dialog" value={JSON.stringify(form?.dialog)} />
			<div class="flex gap-2">
				<input type="text" placeholder="Message" name="question" autocomplete="off" required />
				<button disabled={loading}> Submit </button>
			</div>
		</form>
	</section>
</div>

<style lang="scss">
	.head {
		display: flex;
		gap: 1rem;
	}
</style>
