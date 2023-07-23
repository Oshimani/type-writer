<script lang="ts">
	import { challenges } from "./assets/challenges"
	import Input from "./lib/Input.svelte"

	let challenge = challenges[Math.floor(Math.random() * challenges.length)]
</script>

<nav class="container mx-auto text-center">
	<h1 class="text-5xl">type-writer</h1>
</nav>

<main class="container mx-auto">
	<!-- OUTLET -->
	<Input challengeString={challenge.challengeString} />
</main>

<style>
</style>
