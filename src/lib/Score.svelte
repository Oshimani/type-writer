<script lang="ts">
	import { fade } from "svelte/transition"
	import PrimaryButton from "./PrimaryButton.svelte"

	export let startNewGame: () => void
</script>

<div in:fade={{delay:300,duration:300}} class="h-full text-center flex flex-col justify-center gap-4">
	<h1 class="text-4xl">Game Over!</h1>
	<PrimaryButton onClick={startNewGame} label="Play Again" />
</div>
