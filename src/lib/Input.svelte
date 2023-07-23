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

	function initializeChallengeString(inputString: string) {
		return Array.from(inputString).map(
			(char) => ({ char, status: "initial" } as { char: string; status: CharStatus })
		)
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

<div class="mt-8">
	{#each template as { char, status }, i}
		<Char {char} {status} />
	{/each}
</div>

<div>
	<PrimaryButton onClick={handleClickReset} label="Reset" />
</div>

<style></style>
