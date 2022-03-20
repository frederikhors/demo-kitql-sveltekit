<script lang="ts">
	import { page } from '$app/stores';
	import { KQL_AllContinents, KQL_AllCountriesOfContinent } from '$lib/kitql/generated/stores';
	import { onMount } from 'svelte';

	const continentForPatch = $KQL_AllContinents.data?.continents.find(
		(c) => c.code === $page.params.code
	);

	// console.log('continentForPatch:', continentForPatch);

	// KQL_AllCountriesOfContinent.patch({ continent: continentForPatch });

	onMount(() => {
		KQL_AllCountriesOfContinent.query({ variables: { code: $page.params.code } });
		// No need this here. Global setting available here: src/lib/kitql/kitQLClient.ts
		// setTimeout(() => {
		// 	// I'm using this to simluate a backend delay
		// }, 5000);
	});
</script>

<h1 class="text-xl">
	<a href="/continents">&laquo; Go back</a> | Continent:
	<b>{$KQL_AllCountriesOfContinent.data.continent?.name}</b>
	({$KQL_AllCountriesOfContinent.data.continent?.code})
</h1>

<div class="grid grid-cols-3 mt-4">
	<div class="col-span-2">
		{#if $KQL_AllCountriesOfContinent.isFetching}
			Loading...
		{:else if $KQL_AllCountriesOfContinent.errors}
			{#each $KQL_AllCountriesOfContinent.errors as error}
				{error}
			{/each}
		{:else}
			<ul>
				{#each $KQL_AllCountriesOfContinent.data.continent?.countries || [] as country}
					<li class="my-3 text-lg">
						{country.code} - {country.name}
					</li>
				{:else}
					No countries
				{/each}
			</ul>
		{/if}
	</div>

	<pre class="whitespace-pre-wrap text-xs">{JSON.stringify(
			$KQL_AllCountriesOfContinent,
			null,
			' '
		)}</pre>
</div>
