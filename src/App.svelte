<script lang="ts">
	import { challenges } from "./assets/challenges"
	import Game from "./lib/Game.svelte"
	import Score from "./lib/Score.svelte"
	import type { Results } from "./models/results"

	let challenge = challenges[Math.floor(Math.random() * challenges.length)]

	let component
	let props

	function showGame() {
		;(component = Game),
			(props = {
				challengeString: challenge.challengeString,
				gameOver: showScore,
				time: 60
			})
	}
	function showScore(results: Results) {
		;(component = Score),
			(props = {
				startNewGame: showGame,
				results
			})
	}

	showGame()
</script>

<main class="container mx-auto h-screen flex flex-col items-center">
	<h1 class="text-5xl">type-writer</h1>

	<!-- OUTLET -->
	<svelte:component this={component} {...props} />
</main>

<!-- test -->

<style>
</style>
