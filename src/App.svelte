<script lang="ts">
	import { challenges } from "./assets/challenges"
	import Game from "./lib/Game.svelte"
	import GitHub from "./lib/GitHub.svelte"
	import PrimaryButton from "./lib/PrimaryButton.svelte"
	import Score from "./lib/Score.svelte"
	import type { Results } from "./models/results"

	let challenge = challenges[Math.floor(Math.random() * challenges.length)]
	let time = 30

	let component
	let props

	function showGame() {
		component = Game
		props = {
			challengeString: challenge.challengeString,
			gameOver: showScore,
			time
		}
	}
	function showScore(results: Results) {
		component = Score
		props = {
			startNewGame: showGame,
			results
		}
	}

	function updateTime(newTime: number) {
		time = newTime
		props.time = time
		props = props
	}

	showGame()
</script>

<main class="container mx-auto h-screen flex flex-col items-center gap-4">
	<h1 class="text-5xl">type-writer</h1>

	<div class="flex flex-row gap-4">
		<PrimaryButton disabled={time === 15} onClick={() => updateTime(15)}>15 s</PrimaryButton>
		<PrimaryButton disabled={time === 30} onClick={() => updateTime(30)}>30 s</PrimaryButton>
		<PrimaryButton disabled={time === 60} onClick={() => updateTime(60)}>60 s</PrimaryButton>
		<PrimaryButton disabled={time === 120} onClick={() => updateTime(120)}>120 s</PrimaryButton>
	</div>

	<!-- OUTLET -->
	<svelte:component this={component} {...props} />
</main>

<GitHub />

<style>
</style>
