<script context="module" lang="ts">
	import { KQL_AllCountriesOfContinent } from '$lib/kitql/generated/stores';

	export async function load({ fetch, url, params, session, stuff }) {
		await KQL_AllCountriesOfContinent.query({ fetch, variables: { code: params.code } });
		return {};
	}
</script>

<script lang="ts">
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
