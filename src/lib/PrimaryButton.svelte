<script lang="ts">
	export let label: string
	export let onClick: () => void
</script>

<button
	class="border-orange-500 border-solid border-2 px-4 py-1 uppercase font-bold text-orange-500 hover:text-zinc-900 hover:bg-orange-500 transition-colors duration-150 ease-in-out"
	type="button"
	on:click={onClick}>{label}</button
>
