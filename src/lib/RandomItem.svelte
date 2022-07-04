<script lang="ts">
	const items = ['Extra point', 'Extra socket', 'Scissors'];

	let item = items[Math.floor(Math.random() * items.length)];
</script>

<h1>{item}</h1>
<img src="" alt={item} hidden />

<style>
	h1 {
		font-size: 40pt;
		font-family: monospace;
		text-align: center;
	}
</style>
