<script>
	import '../app.css';
</script>

<div class="app">
	<main>
		<slot />
	</main>
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
		background-color: #00f;
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		justify-content: start;
		align-items: center;
		padding: 1rem;
		width: 100%;
		width: 100%;
		margin: 0 auto;
		box-sizing: border-box;
	}
</style>
