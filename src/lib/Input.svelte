<script lang="ts">
	import type { CharStatus } from "../models/char-status"
	import Char from "./Char.svelte"
	import PrimaryButton from "./PrimaryButton.svelte"

	export let challengeString: string = "Hello World!"

	let template = initializeChallengeString(challengeString)
	let currentIndex = 0

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
			wordsArray.sort((a, b) => Math.random() - 0.5)

			inputString = wordsArray.join(" ")
		}

		const arr = Array.from(inputString).map(
			(char) => ({ char, status: "initial" } as { char: string; status: CharStatus })
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

	window.removeEventListener("keydown", onKeyDown)
	window.addEventListener("keydown", onKeyDown)
</script>

<div class="mt-10 relative min-h-full">
	<div class="absolute left-1/2 top-0">
		<div
			style={`transform: translateX(-${11 * currentIndex + 1}px)`}
			class="flex flex-row transition-transform duration-150 ease-in-out"
		>
			{#each template as { char, status }, i}
				<Char {char} {status} />
			{/each}
		</div>
	</div>
</div>

<div class="mt-10 text-center">
	<PrimaryButton onClick={handleClickReset} label="Reset" />
</div>

<style></style>
