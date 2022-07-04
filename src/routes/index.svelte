<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
	import Question from '$lib/Question.svelte';

	let game = { started: false };

	function startGame(): void {
		game.started = true;
	}
</script>

<svelte:head>
	<title>Techcards</title>
	<meta name="description" content="Techcards, an ETEC Game." />
</svelte:head>

<section>
	<h1>Techcards</h1>
	{#if !game.started}
		<div class="init" on:click={startGame}>Start Game</div>
	{:else}
		<Question />
	{/if}
</section>

<style>
	.init {
		border-radius: 12pt;
		background-color: #57bac9;
		color: #135f6b;
		font-family: Arial, Helvetica, sans-serif;
		font-size: xx-large;
		margin: 5% auto;
		padding: 4%;
		text-align: center;
		width: 60%;
	}
</style>
