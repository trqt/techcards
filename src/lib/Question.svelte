<script context="module" lang="ts">
</script>

<script lang="ts">
	import QuestionStore from '../store/QuestionsStore';
	import RandomItem from './RandomItem.svelte';
	import Timer from './Timer.svelte';
	let quest: {
		question: string;
		correct: boolean;
	};

	let questions = QuestionStore;

	$: state = 'question';

	function pickRandomQuestion(): void {
		const rand = Math.floor(Math.random() * questions.length);
		const tmp = questions.at(rand);
		console.log(`Picking new question ${questions.length}`);
		if (questions.length == 0) return;

		if (tmp == undefined || quest == tmp) {
			pickRandomQuestion();
		} else {
			quest = tmp;
			// Dont repeat questions
			questions.splice(questions.indexOf(quest), 1);
		}
	}
	function skip(): void {
		state = 'skipped';
		pickRandomQuestion();
	}
	function check(correct: boolean, pressed: boolean): void {
		if (correct == pressed) {
			state = 'correct';
		} else {
			state = 'wrong';
		}
		pickRandomQuestion();
	}

	function passTurn(): void {
		state = 'question';
	}

	function timeIsOver(event: CustomEvent<any>): void {
		// Doesnt change to 'time is over' state when in ok
		if (event.detail.timeIsOver && state == 'question') {
			console.log('Times over');
			state = 'time is over';
			pickRandomQuestion();
		}
	}

	pickRandomQuestion();
</script>

<section>
	{#if state == 'question'}
		<h2>{quest.question}</h2>

		<div class="answer" on:click={() => check(quest.correct, true)}>Yes</div>
		<div class="answer" on:click={() => check(quest.correct, false)}>No</div>
		<Timer on:message={timeIsOver} />
		<div class="giveup" on:click={skip}>Skip</div>
	{:else if state == 'correct'}
		<h2 class="atext">Correct! You received:</h2>
		<RandomItem />
		<div class="next-turn" on:click={passTurn}>Next question</div>
	{:else if state == 'wrong'}
		<h2 class="atext">Incorrect 😢</h2>
		<div class="next-turn" on:click={passTurn}>Next question</div>
	{:else if state == 'time is over'}
		<h2 class="atext">Turtle! hahaha</h2>
		<div class="next-turn" on:click={passTurn}>Next question</div>
	{:else if state == 'skipped'}
		<h2 class="atext">Skipped</h2>

		<div class="next-turn" on:click={passTurn}>Next question</div>
	{/if}
</section>

<style>
	h2 {
		text-align: center;
	}
	.answer {
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
	.giveup {
		margin: 5% auto;
		padding: 3%;
		text-align: center;
		width: 30%;
		color: #b51212;
		background-color: #e27272;
		border-radius: 5pt;
		font-family: Arial, Helvetica, sans-serif;
	}
	.next-turn {
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
