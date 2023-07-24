<script lang="ts">
	import { twMerge } from "tailwind-merge"
	import type { CharStatus } from "../models/char-status"
	import type { Results } from "../models/results"
	import Char from "./Char.svelte"
	import PrimaryButton from "./PrimaryButton.svelte"
	import Timer from "./Timer.svelte"

	export let challengeString: string = "Hello World!"
	export let time: number = 30
	export let gameOver: (results: Results) => void

	let template = initializeChallengeString(challengeString)

	let avgWordLength =
		challengeString
			.split(" ")
			.map((word) => word.length)
			.reduce((a, b) => a + b, 0) / challengeString.split(" ").length

	let currentIndex = 0
	let numberOfErrors = 0

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
			numberOfErrors++
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
		numberOfErrors = 0
		template = initializeChallengeString(challengeString)
	}

	function handleClickReset() {
		reset()
	}

	function handleTimeUp() {
		gameOver({
			challengeInfo: {
				avgWordLength,
				time
			},
			numberOfErrors,
			numberOfUncorrectedErrors: template.filter(({ status }) => status === "incorrect").length,
			numberOfDistinctCorrectChars: template.filter(({ status }) => status === "correct").length
		})
	}

	window.removeEventListener("keydown", onKeyDown)
	window.addEventListener("keydown", onKeyDown)
</script>

<div class="flex flex-col gap-16 h-full w-full my-10 overflow-x-hidden">
	<!-- SCROLLING TEXT CONTAINER -->
	<div class="relative flex-grow">
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
		<div
			class={twMerge(
				"absolute right-0 top-1/2 bg-gradient-to-r from-transparent   w-1/2 h-8",
				"to-white",
				"dark:to-zinc-900"
			)}
		/>
		<div
			class={twMerge("absolute h-8 right-0 top-1/2full-mask right", "bg-white", "dark:bg-zinc-900")}
		/>
		<!-- LEFT -->
		<div
			class={twMerge(
				"absolute left-0 top-1/2 bg-gradient-to-l from-transparen w-1/2 h-8",
				"to-white",
				"dark:to-zinc-900"
			)}
		/>
		<div
			class={twMerge("absolute left-0 top-1/2  h-8 full-mask left", "bg-white", "dark:bg-zinc-900")}
		/>
	</div>

	<!-- TIMER -->
	<Timer running={gameRunning} {time} onTimeUp={handleTimeUp} />

	<!-- BUTTONS -->
	<PrimaryButton onClick={handleClickReset}>Reset</PrimaryButton>
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
