<svelte:head>
	<title>Data Bit 1</title>
</svelte:head>

<script>
	import Scroller from './Scroller.svelte';
	import { onMount } from 'svelte';
	import Papa from 'papaparse';
	import * as PlotLib from '@observablehq/plot';
	import Nested1 from './Nested1.svelte';
	import Nested2 from './Nested2.svelte';
	import { base } from '$app/paths';

	/** @type {{ year: number; immigration_total: number; source?: string; estimate?: string; link?: string }[]} */
	let data = $state([]);

	let chartEl = $state();
	let visibleProgress = $state(0);

	const sections = [
		{
			text: '"These foreigners in an irregular situation are a source of crime, drugs and several other scourges. We are not telling the authorities to throw these migrants into the sea or beyond the deserts. But staying in Algeria must follow rules. We will not allow the Algerian people to suffer from disorder." - Prime Minister Ouyahyia 2017'
		},
		{
			text: '"Algeria’s expulsions to Niger are not necessarily new. They have been taking place regularly since 2017, and they also occurred before that, but since 2017 they have happened at a much higher frequency […] People who have been in the country longer know in which months they should not leave the house." - Journalist from Algiers 2023'
		},
		{
			text: '"In October 2023, to quote a recurrent expression used by people on the move,<i> the sea gate was closed</i>. Indeed, when Tunisia shut its northern borders and a military coup unfolded in Niger, migration routes shifted abruptly—pushing more movement toward Algeria and coinciding with a sharp rise in arrests." - Anthropologist based in Tunis 2026'
		}
	];

	onMount(async () => {
		try {
			const res = await fetch(`${base}/data/df_immigration_2015til2025_full_sources.csv`);
			const text = await res.text();

			const parsed = Papa.parse(text, {
				header: true,
				delimiter: ';',
				skipEmptyLines: true
			});

			data = parsed.data
				.map(
					/** @param {{ year: string; immigration_total: string; source?: string; estimate?: string; link?: string }} d */
					(d) => ({
						year: Number(d.year),
						immigration_total: Number(d.immigration_total),
						source: d.source,
						estimate: d.estimate,
						link: d.link
					})
				)
				.sort((a, b) => a.year - b.year);
		} catch (err) {
			console.error('Failed to load CSV:', err);
		}
	});

	function syncProgress(progress) {
		visibleProgress = progress;
		return '';
	}

	$effect(() => {
		if (!chartEl || data.length === 0) return;

		const visibleData = data.slice(
			0,
			Math.max(2, Math.floor(visibleProgress * data.length))
		);

		chartEl.innerHTML = '';

		const plot = PlotLib.plot({
			width: 900,
			height: 500,
			marginLeft: 80,
			marginRight: 20,
			marginTop: 20,
			marginBottom: 60,
			x: {label: 'Year', ticks: [2015,2016,2017,2018,2019,2020,2021,2022,2023,2024,2025], 
				tickFormat: (d) => String(d)
				},
			y: {
				label: 'Number of immigrants arrested and deported',
				grid: true
			},
			marks: [
				PlotLib.line(visibleData, {
					x: 'year',
					y: 'immigration_total',
					stroke: 'rgb(99, 15, 9)',
					strokeWidth: 4
				})
			]
		});

		chartEl.append(plot);

		return () => {
			plot.remove();
		};
	});
</script>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		background-color: #f7f7f7e3;
		font-family: 'Arial', sans-serif;
		line-height: 1.8;
	}

	:global(main) {
		max-width: 1000px;
		margin: 0 auto;
		padding: 2rem 2rem 4rem 2rem;
	}

	h1 {
		text-align: center;
		color: rgb(99, 15, 9);
		font-family: 'Corbel', sans-serif;
		font-size: 5rem;
		line-height: 1.2;
		margin-bottom: 4rem;
	}

	p {
		font-size: 1.1rem;
		line-height: 1.5;
		margin-bottom: 1.2rem;
		color: #222;
	}

	.chart-wrap {
		max-width: 900px;
		margin: 3rem auto 6rem auto;
		padding: 0;
		background: transparent;
		border: none;
		box-shadow: none;
		min-height: 450px;
	}
</style>

<h1>Inside Algeria&apos;s Migration Crackdown</h1>

<Nested1 />

<Scroller {sections} threshold={0.8}>
	{#snippet background(
		/** @type {{ progress: number }} */ { progress }
	)}
		<div class="background">
			<div class="chart-wrap">
				{syncProgress(progress)}
				{#if data.length > 0}
					<div bind:this={chartEl}></div>
				{:else}
					<p>Loading chart...</p>
				{/if}
			</div>
		</div>
	{/snippet}
</Scroller>

<Nested2 />