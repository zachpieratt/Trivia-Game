<script>
	import he from 'he';
	import { onMount } from 'svelte';

	const endpoint = 'https://opentdb.com/api.php?amount=1';
	let questions = [];

	onMount(async function () {
		const response = await fetch(endpoint, { method: 'GET', mode: 'cors' });
		const data = await response.json();
		questions = data.results;
		console.log(questions);
	});
</script>

<div class="header">
	<h1>Trivia</h1>
	<a href="/about">About my site</a>
</div>
<div class="body">
	{#each questions as question}
		<div>{he.decode(question.question)}</div>
	{/each}
</div>

<style>
	.header {
		text-align: center;
	}
</style>
