<script lang="ts">
	import Header from "@/components/Header.svelte";
	import Finish from "@/components/Finish.svelte";
	import Question from "@/components/Question.svelte";
	import Progress from "@/components/Progress.svelte";
	import Layout from "@/components/Layout.svelte";

	import { questions } from "@/config";

	let question_index = 0;
	let finished = false;
	let evaluation: boolean[] = [];

	$: question = questions[question_index];

	function finish_question(e: CustomEvent<boolean>): void {
		const is_correct = e.detail;
		evaluation.push(is_correct);
		update_question();
	}

	function update_question(): void {
		if (question_index < questions.length - 1) {
			question_index++;
		} else {
			finished = true;
		}
	}

	function restart(): void {
		question_index = 0;
		finished = false;
		evaluation = [];
	}
</script>

<Header />
<Layout>
	{#if !finished}
		<Progress
			progress={question_index}
			count={questions.length}
		/>
		<Question {question} on:finish={finish_question} />
	{:else}
		<Finish {evaluation} on:restart={restart} />
	{/if}
</Layout>
