<script lang="ts">
	import type { CharStatus } from "../models/char-status"
	import Char from "./Char.svelte"
	import PrimaryButton from "./PrimaryButton.svelte"
	import Timer from "./Timer.svelte"

	export let challengeString: string = "Hello World!"

	let template = initializeChallengeString(challengeString)
	let currentIndex = 0

	$: gameRunning = currentIndex > 0

	function isModKey(key: string) {
		return key === "Shift" || key === "Control" || key === "Alt"
	}

	function onKeyDown(event: KeyboardEvent) {
		// IS INVALID KEY
		if (isModKey(event.key)) return

		// BACKSPACE
		if (event.key === "Backspace") {
			if (currentIndex === 0) return
			template[currentIndex].status = "initial"
			template[currentIndex - 1].status = "current"
			currentIndex--
			template = template
			return
		}

		// KEY IS ALPHANUMERIC
		if (event.key === template[currentIndex].char) {
			template[currentIndex].status = "correct"
			if (template[currentIndex + 1]) {
				template[currentIndex + 1].status = "current"
			}
		} else {
			template[currentIndex].status = "incorrect"
		}
		if (currentIndex < template.length - 1) {
			template[currentIndex + 1].status = "current"
			currentIndex++
		}
		template = template
	}

	function initializeChallengeString(inputString: string, randomize = true) {
		if (randomize) {
			const wordsArray = inputString.split(" ")
			wordsArray.sort((_) => Math.random() - 0.5)

			inputString = wordsArray.join(" ")
		}

		const arr = Array.from(inputString).map(
			(char) =>
				({ char, status: "initial" }) as {
					char: string
					status: CharStatus
				}
		)
		arr[0].status = "current"
		return arr
	}

	function reset() {
		currentIndex = 0
		template = initializeChallengeString(challengeString)
	}

	function handleClickReset() {
		reset()
	}

  function handleTimeUp() {
    console.log("Time up!")
    gameOver()
  }

  function gameOver(){

  }

	window.removeEventListener("keydown", onKeyDown)
	window.addEventListener("keydown", onKeyDown)
</script>

<div class="flex flex-col gap-10 h-full my-10 w-full">
  <!-- SCROLLING TEXT CONTAINER -->
	<div class="relative flex-grow">
		<span class="text-zinc-900 select-none">
			I am inivisible to fix the height of this element :D
		</span>

		<!-- SCROLLING TEXT -->
		<div class="absolute left-1/2 top-1/2">
			<div
				style={`transform: translateX(-${11 * currentIndex + 11 / 2}px)`}
				class="flex flex-row transition-transform duration-150 ease-in-out"
			>
				{#each template as { char, status }, i}
					<Char {char} {status} />
				{/each}
			</div>
		</div>

		<!-- FADE MASKS -->
		<!-- RIGHT -->
		<div class="absolute right-0 top-1/2 bg-gradient-to-r from-transparent to-zinc-900 w-1/2 h-8" />
		<div class="absolute h-8 right-0 top-1/2 bg-zinc-900 full-mask right" />
		<!-- LEFT -->
		<div class="absolute left-0 top-1/2 bg-gradient-to-l from-transparent to-zinc-900 w-1/2 h-8" />
		<div class="absolute left-0 top-1/2 bg-zinc-900 h-8 full-mask left" />
	</div>

	<!-- TIMER -->
	<Timer running={gameRunning} time={15} onTimeUp={handleTimeUp} />

	<!-- BUTTONS -->
	<PrimaryButton onClick={handleClickReset} label="Reset" />
</div>

<style>
	.full-mask {
		width: 9999px;
	}
	.left {
		transform: translateX(-9999px);
	}
	.right {
		transform: translateX(9999px);
	}
</style>
