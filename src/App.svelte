<script lang="ts">
	import { challenges } from "./assets/challenges"
	import Input from "./lib/Input.svelte"

	let challenge = challenges[Math.floor(Math.random() * challenges.length)]
</script>

<main class="container mx-auto h-screen flex flex-col items-center">
	<h1 class="text-5xl">type-writer</h1>

	<!-- OUTLET -->
	<Input challengeString={challenge.challengeString} />
</main>

<style>
</style>
