<script lang="ts">
	import { twMerge } from "tailwind-merge"
	import type { CharStatus } from "../models/char-status"

	export let char: string
	export let status: CharStatus
</script>

<span
	style="min-width: 11px;"
	class={twMerge(
		"font-mono text-xl transition-colors duration-150 ease-in-out",
		status === "initial" && "dark:text-gray-400 text-gray-600",
		status === "correct" && "dark:text-orange-500 text-orange-600",
		status === "incorrect" && "text-gray-200 bg-red-600",
		status === "current" && "dark:text-gray-100 text-gray-900 dark:bg-gray-600 bg-gray-300"
	)}>{char}</span
>
