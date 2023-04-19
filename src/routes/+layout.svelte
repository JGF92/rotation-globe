<script>
	import Header from './Header.svelte';
	import Hero from './Hero.svelte';		
	import './styles.css';
</script>

<div class="app">
	<Header/>
	<Hero/>
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
		background-color :#FFF1E0;
	}
</style>

<!-- background-color :#FFF1E0; -->