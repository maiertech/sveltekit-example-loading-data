<script>
	import { onMount } from 'svelte';

	let promise;

	async function load() {
		const response = await fetch('https://swapi.dev/api/people/1', {
			headers: { Accept: 'application/json' }
		});
		const data = await response.json();
		return data;
	}

	onMount(() => {
		promise = load();
	});
</script>

{#await promise}
	Loading...
{:then data}
	<pre>{JSON.stringify(data, null, 2)}</pre>
{/await}
