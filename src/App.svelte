<script lang="ts">
	import { challenges } from "./assets/challenges"
	import Game from "./lib/Game.svelte"
	import Score from "./lib/Score.svelte"

	let challenge = challenges[Math.floor(Math.random() * challenges.length)]

	let component
	let props

	function showGame() {
		;(component = Game),
			(props = {
				challengeString: challenge.challengeString,
				gameOver: showScore
			})
	}
	function showScore() {
		;(component = Score),
			(props = {
				startNewGame: showGame
			})
	}

	showGame()
</script>

<main class="container mx-auto h-screen flex flex-col items-center">
	<h1 class="text-5xl">type-writer</h1>

	<!-- OUTLET -->
	<svelte:component this={component} {...props} />
</main>

<style>
</style>
