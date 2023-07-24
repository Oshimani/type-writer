<script lang="ts">
	import { twMerge } from "tailwind-merge"

	export let label: string
	export let onClick: () => void
</script>

<button
	class={twMerge(
		"border-solid border-2 px-4 py-1 uppercase font-bold transition-colors duration-150 ease-in-out",
		"border-orange-600 text-orange-600 hover:text-white hover:bg-orange-600",
		"dark:border-orange-500 dark:text-orange-500 dark:hover:text-zinc-900 dark:hover:bg-orange-500"
	)}
	type="button"
	on:click={onClick}>{label}</button
>
