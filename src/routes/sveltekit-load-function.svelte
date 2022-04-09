<script context="module">
	export async function load({ fetch }) {
		const response = await fetch('https://swapi.dev/api/people/1', {
			headers: { Accept: 'application/json' }
		});
		const data = await response.json();
		return {
			props: { data }
		};
	}
</script>

<script>
	export let data;
</script>

<pre>{JSON.stringify(data, null, 2)}</pre>
