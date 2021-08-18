<script lang="ts">
	import { API_KEY, API_URL } from '$lib/vars.js';
	import { Circle } from 'svelte-loading-spinners';
	import Photo from '$lib/Photo.svelte';
	import Carousel from 'svelte-carousel';

	interface Photo {
		img_src: string;
		[key: string]: any;
	}

	let photos: Promise<Array<Photo>>;

	let selectedDate = '2021-08-15';

	// $: console.log(selectedDate);

	// $: console.log(photos);

	$: if (selectedDate) {
		photos = fetch(`${API_URL}?earth_date=${selectedDate}&api_key=${API_KEY}`)
			.then((r) => r.json())
			.then((r) => {
				console.log(r.photos);
				return r.photos;
			});
	}
</script>

<head>
	<title>Mars Explorer</title>
	<meta
		name="description"
		content="See pictures captured by the Curiosity Rover on an Earth day of your choice"
	/>
	<meta name="author" content="Dhaiwat Pandya (@dhaiwat10)" />
</head>

<main>
	<h1 class="title">Mars explorer</h1>
	<p class="subtitle">Pictures captured by the Curiosity Rover on an Earth day of your choice</p>

	{#if photos}
		{#await photos}
			<div class="spinner-container">
				<Circle size={40} />
			</div>
		{:then photos}
			{#if photos.length < 1}
				<div class="spinner-container">
					<p>No photos found for this date.</p>
				</div>
			{:else}
				<Carousel dots={false}>
					{#each photos as photo}
						<Photo src={photo.img_src} />
					{/each}
				</Carousel>
			{/if}
		{/await}
	{/if}

	<form>
		<label for="date-picker">Select a date</label>
		<input
			name="date-picker"
			class="date-picker"
			type="date"
			bind:value={selectedDate}
			max={new Date().toISOString().substring(0, 10)}
		/>
	</form>

	<footer>
		<a href="https://github.com/dhaiwat10/mars-explorer" target="_blank"
			><img class="gh-logo" src="gh.png" alt="Github repo" /></a
		>Made by&nbsp; <a href="https://twitter.com/dhaiwat10" target="_blank">Dhaiwat</a>
	</footer>
</main>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap');

	.title {
		margin: 0.5rem auto;
	}

	.subtitle {
		margin-top: 0;
		margin-bottom: 2rem;
	}

	.spinner-container {
		justify-content: center;
		align-items: center;
		display: flex;
		margin-bottom: 2rem;
		height: 60vh;
	}
	main {
		text-align: center;
		font-family: 'Inter', sans-serif !important;
	}

	form {
		display: flex;
		gap: 1rem;
		justify-content: center;
		align-items: center;
	}
	.date-picker {
		padding: 0.5rem;
		border-radius: 7px;
		border: 1px solid #ccc;
		cursor: pointer;
	}

	/* hr {
		margin: 2rem auto;
		opacity: 0.1;
	} */

	footer {
		margin-top: 2rem;
		padding: 1rem;
		display: flex;
		align-items: center;
		justify-content: center;
		border-top: 1px solid #ccc;
	}

	.gh-logo {
		height: 2rem;
		margin-right: 1rem;
	}
</style>
