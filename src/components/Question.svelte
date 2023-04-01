<script lang="ts">
	import { createEventDispatcher } from "svelte";

	export let question: question;

	const dispatch = createEventDispatcher<{ finish: boolean }>();

	let selected_index = 0;
	let message = "";
	let submitted = false;
	let correct = false;

	function check_answer() {
		submitted = true;

		correct = selected_index == question.correct_answer_index;

		const correct_answer =
			question.answers[question.correct_answer_index];

		message = correct
			? "This is correct!"
			: "No. The correct answer is: " + correct_answer;
	}

	function next() {
		dispatch("finish", correct);
		selected_index = 0;
		message = "";
		submitted = false;
		correct = false;
	}
</script>

<form
	on:submit|preventDefault={check_answer}
	role="region"
	aria-label="question"
>
	<h2 aria-live="assertive">
		{question.question}
	</h2>

	<section class="answers" aria-label="answers">
		{#each question.answers as answer, index}
			<label
				class="answer"
				class:selected={index == selected_index}
			>
				<input
					type="radio"
					value={index}
					name={question.id}
					disabled={submitted}
					bind:group={selected_index}
				/>
				<span class="answer_text">
					{answer}
				</span>
			</label>
		{/each}
	</section>

	<menu aria-label="menu">
		<button disabled={submitted}>Submit</button>
		{#if submitted}
			<button type="button" on:click={next}> Next </button>
		{/if}
	</menu>
</form>

<div class="message" aria-live="assertive">
	{message}
</div>

<style>
	h2 {
		padding-block: 2rem;
	}

	.answers {
		display: flex;
		flex-direction: column;
		gap: 0.75rem;
	}

	.answer {
		background-color: var(--answer-color);
		padding: 1rem;
		border-radius: 0.25rem;
		font-size: 1.25rem;
		cursor: pointer;
	}

	.answer.selected {
		background-color: var(--answer-color-selected);
		outline: 0.1rem solid var(--outline-color);
	}

	.answer_text {
		padding-left: 0.5rem;
	}

	menu {
		margin-top: 1.5rem;
		display: flex;
		gap: 0.5rem;
	}

	.message {
		font-size: 1.25rem;
		margin-top: 1.5rem;
	}
</style>
