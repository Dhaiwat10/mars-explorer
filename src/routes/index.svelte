<script lang="ts">
	import { API_KEY, API_URL } from '$lib/vars.js';

	let photo;

	let selectedDate;

	$: console.log(selectedDate);

	$: console.log(photo);

	$: if (selectedDate) {
		photo = fetch(`${API_URL}?earth_date=${selectedDate}&api_key=${API_KEY}`)
			.then((r) => r.json())
			.then((r) => r.photos[0].img_src);
	}
</script>

<h1>Mars explorer</h1>

{#if photo}
	{#await photo then src}
		<div>
			<img class="photo" {src} alt="Mars captured on this day" />
		</div>
	{/await}
{/if}

<form>
	<input type="date" bind:value={selectedDate} max={new Date().toISOString().substring(0, 10)} />
</form>

<style>
	.photo {
		width: 60%;
	}
</style>
