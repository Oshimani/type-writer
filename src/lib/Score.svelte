<script lang="ts">
	import { fade } from "svelte/transition"
	import PrimaryButton from "./PrimaryButton.svelte"
	import type { Results } from "../models/results"

	export let startNewGame: () => void
	export let results: Results

	$: wpm =
		results.numberOfDistinctCorrectChars /
		results.challengeInfo.avgWordLength /
		(results.challengeInfo.time / 60)
</script>

<div
	in:fade={{ delay: 300, duration: 300 }}
	class="h-full text-center flex flex-col justify-center gap-4"
>
	<h1 class="text-4xl">Game Over!</h1>
	<h2 class="text-2xl">
		Words per minute: <span class="text-green-500 font-bold">{Math.round(wpm)}</span>
	</h2>
	<p>
		Number of correct characters: <span class="text-green-500 font-bold"
			>{results.numberOfDistinctCorrectChars}</span
		>
	</p>
	<p>
		Number of typos in final text: <span class="text-red-500 font-bold"
			>{results.numberOfUncorrectedErrors}</span
		>
	</p>
	<p>
		Number of incorrect inputs: <span class="text-red-500 font-bold">{results.numberOfErrors}</span>
	</p>
	<PrimaryButton onClick={startNewGame}>Play Again</PrimaryButton>
</div>
