<script lang="ts">
	import ChatMessage from '$lib/ChatMessage.svelte';
	import type { ChatCompletionRequestMessage } from 'openai';
	import { SSE } from 'sse.js';

	let query: string = '';
	let answer: string = '';
	let loading: boolean = false;
	let chatMessages: ChatCompletionRequestMessage[] = [];
	let scrollToDiv: HTMLDivElement;

	function scrollToBottom() {
		setTimeout(function () {
			scrollToDiv.scrollIntoView({ behavior: 'smooth', block: 'end', inline: 'nearest' });
		}, 100);
	}

	const handleSubmit = async () => {
		loading = true;
		chatMessages = [...chatMessages, { role: 'user', content: query }];

		const eventSource = new SSE('/api/chat', {
			headers: {
				'Content-Type': 'application/json'
			},
			payload: JSON.stringify({ messages: chatMessages })
		});

		query = '';

		eventSource.addEventListener('error', handleError);

		eventSource.addEventListener('message', (e) => {
			scrollToBottom();
			try {
				loading = false;
				if (e.data === '[DONE]') {
					chatMessages = [...chatMessages, { role: 'assistant', content: answer }];
					answer = '';
					return;
				}

				const completionResponse = JSON.parse(e.data);
				const [{ delta }] = completionResponse.choices;

				if (delta.content) {
					answer = (answer ?? '') + delta.content;
				}
			} catch (err) {
				handleError(err);
			}
		});
		eventSource.stream();
		scrollToBottom();
	};

	function handleError<T>(err: T) {
		loading = false;
		query = '';
		answer = '';
		console.error(err);
	}
</script>

<h1>Chat GPT</h1>
<div>
	<ChatMessage type="assistant" message="Hello, ask me anything you want!" />
	{#each chatMessages as message}
		<ChatMessage type={message.role} message={message.content} />
	{/each}
	{#if answer}
		<ChatMessage type="assistant" message={answer} />
	{/if}
	{#if loading}
		<ChatMessage type="assistant" message="Loading.." />
	{/if}
</div>
<div class="" bind:this={scrollToDiv} />

<form on:submit|preventDefault={() => handleSubmit()}>
	<input type="text" bind:value={query} />
	<button type="submit">Chat</button>
</form>
