<script lang="ts">
	import { createEventDispatcher } from "svelte";
	const dispatch = createEventDispatcher<{ restart: undefined }>();

	export let evaluation: boolean[] = [];

	const count = evaluation.length;
	const correct_count = evaluation.filter((x) => x).length;

	function restart(): void {
		dispatch("restart");
	}
</script>

<section>
	<h2>The quiz is finished</h2>
	<p>
		Out of {count} questions you answered
		{correct_count} correctly.
	</p>
	<menu aria-label="menu">
		<button on:click={restart}>Start over</button>
	</menu>
</section>

<style>
	p {
		font-size: 1.25rem;
		padding-block: 2rem 1.5rem;
	}
</style>
