<script>
	import SvelteScroller from '@sveltejs/svelte-scroller';
	import { innerHeight } from 'svelte/reactivity/window';

	let { threshold = 0.8, sections = [], background: scrollBg } = $props();

	let bgHeight = $state(0);
	let count = $state(-1);
	let index = $state(0);
	let offset = $state(0);
	let progress = $state(0);

	const viewportHeight = $derived(innerHeight.current ?? 800);

	const top = $derived((viewportHeight - bgHeight) / 2 / viewportHeight);
	const bottom = $derived(1 - (viewportHeight - bgHeight) / 2 / viewportHeight);
</script>

<SvelteScroller
	{top}
	{bottom}
	{threshold}
	bind:count
	bind:index
	bind:offset
	bind:progress
>
	{#snippet background()}
		<div bind:clientHeight={bgHeight}>
			{@render scrollBg({ index, count, offset, progress, section: sections[index] })}
		</div>
	{/snippet}

	{#snippet foreground()}
		<div class="foreground">
			{#each sections as section}
				<section>{@html section.text}</section>
			{/each}
		</div>
	{/snippet}
</SvelteScroller>

<style>
	.foreground {
		pointer-events: none;
		display: flex;
		flex-direction: column;
		gap: 60vh;
		max-width: 350px;
		margin: 5rem auto;
	}

	.foreground section {
			pointer-events: all;
			max-width: 600px;
			margin: 0;
			font-family: 'Corbel', sans-serif;
			font-size: 1 rem;
			line-height: 1;
			color: #222;
			background-color: rgba(255, 255, 255, 0.95);
			padding: 1.4rem 1.6rem;
			border-radius: 4px;
			box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
			transition: all 0.3s ease;
		}

	.foreground section:first-child {
		margin-top: 60vh;
	}

	.foreground section:last-child {
		margin-bottom: 100vh;
	}
</style>