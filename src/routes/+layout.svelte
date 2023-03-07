<script lang="ts">
	import Header from '$lib/Header.svelte';
	import '@picocss/pico';
	import '../styles.scss';
	import { navigating } from '$app/stores';
	import FadeTransition from '$lib/FadeTransition.svelte';

	$: navigate = ($navigating?.from !== $navigating?.to).toString();
</script>

<svelte:head>
	<title>SvelteKit AI</title>
</svelte:head>

<Header />

<FadeTransition trigger={navigate} start={{ delay: 150, duration: 150 }} stop={{ duration: 150 }}>
	<main class="container">
		<slot />
	</main>
</FadeTransition>
