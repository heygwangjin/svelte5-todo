<script>
	import '../app.css';
</script>

<main>
	<slot />
</main>

<style>
	:global(html, body) {
		height: 100%;
	}

	:global(body) {
		display: grid;
		place-content: center;
		font-family: 'Manrope Variable', sans-serif;
		color: hsl(220 10% 98%);
		background-color: hsl(220 10% 8%);
	}
</style>
