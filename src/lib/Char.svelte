<script lang="ts">
	import { twMerge } from "tailwind-merge"
	import type { CharStatus } from "../models/char-status"

	export let char: string
	export let status: CharStatus
</script>

<span
	class={twMerge(
		"font-mono text-xl transition-colors duration-150 ease-in-out",
		status === "initial" && "text-gray-400",
		status === "correct" && "text-orange-500",
		status === "incorrect" && "text-gray-200 bg-red-600",
		status === "current" && "text-gray-100 bg-gray-600"
	)}>{char}</span
>
