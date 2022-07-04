<script context="module" lang="ts">
</script>

<script lang="ts">
	import { createEventDispatcher, onDestroy } from 'svelte';
	import { tweened } from 'svelte/motion';
	export let original = 40; // TYPE NUMBER OF SECONDS HERE
	let timer = tweened(original);

	const dispatch = createEventDispatcher();

	const tick = setInterval(() => {
		if ($timer > 0) $timer--;

		if ($timer < 1) dispatch('message', { timeIsOver: true });
	}, 1000);
	onDestroy(() => {
		clearInterval(tick);
	});

	$: minutes = Math.floor($timer / 60);
	$: minname = minutes > 1 ? 'minutes' : 'minute';
	$: seconds = Math.floor($timer - minutes * 60);
	$: secname = seconds > 1 ? 'seconds' : 'second';
</script>

<h1>
	{#if $timer > 0}
		You have
		{#if minutes > 0}
			{minutes}
			{minname} and
		{/if}
		{seconds}
		{secname}!
	{/if}
</h1>
<progress value={$timer / original} />

<style>
	progress {
		display: block;

		width: 100%;
	}
	h1 {
		text-align: center;
		margin-top: 10vh;
	}
</style>
