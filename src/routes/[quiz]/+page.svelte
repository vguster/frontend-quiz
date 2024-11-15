<script>
	import { page } from '$app/stores';
	import ListItem from '$lib/fonts/components/ListItem.svelte';
	import { writable } from 'svelte/store';
	let { data } = $props();
	let currentQuestion = $state(0);
	let selectedAnswer = $state(' ');
	let score = $state(0);
	let submitted = $state(false);

	function handleSubmit() {
		submitted = true;
		data.questions[currentQuestion].selectedAnswer = selectedAnswer;
		if (selectedAnswer === data.questions[currentQuestion].answer) {
			score++;
		}
	}

	const progress = writable(0.1);

	function nextQuestion() {
		submitted = false;
		currentQuestion++;
		selectedAnswer = ' ';
		progress.update((n) => {
			if (n + 0.1 > 1) {
				return 1;
			} else {
				return n + 0.1;
			}
		});
	}
</script>

<progress value={$progress}></progress>

{#if currentQuestion < data.questions.length}
	{#each data.questions as question, index (question.question)}
		{#if index === currentQuestion}
			<p class="question-number">Question {index + 1} of {data.questions.length}</p>
			<h2 class="question-text">{question.question}</h2>
			<div class="options-container">
				{#each question.options as option, index (option)}
					{#key submitted}
						<ListItem
							title={option}
							{index}
							correctAnswer={question.selectedAnswer && question.answer === option}
							incorrectlySelected={question.selectedAnswer === option && question.answer !== option}
							focusAction={() => (selectedAnswer = option)}
							disabled={submitted}
						></ListItem>
					{/key}
				{/each}
			</div>
		{/if}
	{/each}

	{#if !submitted}
		<button class="btn btn-primary" onclick={handleSubmit} disabled={!selectedAnswer}>Submit</button
		>
	{:else}
		<button class="btn btn-primary" onclick={nextQuestion}>Next Question</button>
	{/if}
{:else}
	<h2>Game Over!</h2>
	<p>You scored {score} out of {data.questions.length} points!</p>
	<button class="btn btn-primary" onclick={window.location.reload()}>Play again!</button>
{/if}

<style>
	progress {
		display: block;
		border-top-left-radius: 10px;
		border-bottom-left-radius: 10px;
		border-top-right-radius: 10px;
		border-bottom-right-radius: 10px;
		width: 25%;
	}

	.options-container {
		display: flex;
		flex-direction: column;
		gap: 1rem;
	}

	.question-number {
		margin-bottom: 1rem;
	}

	.question-text {
		margin-bottom: 1rem;
	}
</style>
