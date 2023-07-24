<script lang="ts">
	import { twMerge } from "tailwind-merge"

	export let time: number = 60
	export let running: boolean = false
	export let onTimeUp: () => void

	let interval
	let remainingTime = time

	$: {
		if (running) start()
		else {
			stop()
			remainingTime = time
		}
	}

	$: {
		if (remainingTime === 0) {
			onTimeUp()
			stop()
		}
	}

	function start() {
		interval = setInterval(() => {
			remainingTime--
		}, 1000)
	}

	function stop() {
		clearInterval(interval)
	}
</script>

<div class="relative">
	<span class="dark:text-zinc-900 text-white select-none">
		I am inivisible to fix the height of this element :D
	</span>
	<div class="absolute left-1/2 top-0">
		<div
			style={`transform: translateX(-${36 * (time - remainingTime) + 36 / 2}px)`}
			class="flex flex-row transition-transform duration-250 ease-in-out"
		>
			{#each Array(time + 1).fill(0) as _, i}
				<div
					class={twMerge(
						"transition-all duration-250 ease-in-out",
						remainingTime === time - i && "scale-300 text-orange-500"
					)}
				>
					<div class="px-2 font-bold w-9 text-center">
						{time - i}
					</div>
				</div>
			{/each}
		</div>
	</div>

	<!-- FADE MASKS -->
	<div
		class={twMerge(
			"absolute right-1/3 -bottom-8 bg-gradient-to-r from-transparent w-1/6 h-20",
			"to-white",
			"dark:to-zinc-900"
		)}
	/>
	<div
		class={twMerge(
			"absolute h-20 right-1/3 -bottom-8 full-mask right",
			"bg-white",
			"dark:bg-zinc-900"
		)}
	/>
	<!-- LEFT -->
	<div
		class={twMerge(
			"absolute left-1/3 -bottom-8 bg-gradient-to-l from-transparent w-1/6 h-20",
			"to-white",
			"dark:to-zinc-900"
		)}
	/>
	<div
		class={twMerge(
			"absolute left-1/3 -bottom-8 h-20 full-mask left",
			"bg-white",
			"dark:bg-zinc-900"
		)}
	/>
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
	.scale-300 {
		transform: scale(6);
	}
</style>
