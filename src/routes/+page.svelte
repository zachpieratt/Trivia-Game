<script>
	import he from 'he';
	import { onMount } from 'svelte';

	const endpoint = 'https://opentdb.com/api.php?amount=10';
	let questionData = [];

	const questionTypes = {
		boolean: 'True / False',
		multiple: 'Multiple Choice'
	};

	onMount(async function () {
		const response = await fetch(endpoint, { method: 'GET', mode: 'cors' });
		const data = await response.json();
		questionData = data.results;
		for (let i = 0; i < questionData.length; i++) {
			let answerArray = questionData[i].incorrect_answers;
			answerArray.push(questionData[i].correct_answer);
		}
	});
</script>

<div class="header">
	<h1>Trivia</h1>
	<a href="/about">About my site</a>
</div>
<br />
<div class="body">
	{#each questionData as question}
		<div class="questionBox">
			<p class="category">Category : {he.decode(question.category)}</p>
			<p class="difficulty">Difficulty: {question.difficulty.toUpperCase()}</p>
			<div class="questionType">Question Type: {questionTypes[question.type]}</div>
			<br />
			<div class="questionText">{he.decode(question.question)}</div>
			{#if question.type == 'boolean'}
				<button class="trueButton">True</button>
				<button class="falseButton">False</button>
			{/if}
		</div>
	{/each}
</div>

<style>
	.header,
	.difficulty,
	.questionType {
		text-align: center;
	}
	.category {
		text-decoration: underline;
		font-weight: bolder;
		text-align: center;
	}
	.questionBox {
		border: 1px solid black;
	}
</style>
