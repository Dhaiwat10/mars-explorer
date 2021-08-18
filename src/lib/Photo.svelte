<script lang="ts">
	export let src: string;
</script>

<div>
	<img class="photo" {src} alt="Mars on this day" />
</div>

<style>
	.photo {
		height: 60vh;
		margin-bottom: 2rem;
	}
</style>
