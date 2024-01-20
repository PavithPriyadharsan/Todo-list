<script>
	import Header from "./components/Header.svelte";
	import List from "./components/List.svelte";
</script>



<style>
	main{
		background-color: var(--color-grey);
    	font-family: "Roboto", sans-serif;
    	margin: 0;
    	min-height: 80vh;
    	display: flex;
    	justify-content: center;
    	align-items: center;
	}
	.container {
	    width: 500px;
		border-radius: 10px;
		background: grey;
	
	}

	:global(.container *) {
		font-family: sans-serif;
	}
</style>
<main>
	<div class="container">
		<Header></Header>
		<List></List>
	</div>	
</main>

