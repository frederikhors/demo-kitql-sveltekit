<script context="module" lang="ts">
	import { KQL_AllContinents, KQL_AllCountriesOfContinent } from '$lib/kitql/generated/stores';
	import { get } from 'svelte/store';
	import { KitQLInfo } from '@kitql/comp';

	export async function load({ fetch, url, params, session, stuff }) {
		const code = params.code;
		const continentFound = get(KQL_AllContinents).data?.continents.find((c) => c.code === code);

		KQL_AllCountriesOfContinent.patch(
			{
				continent: {
					name: 'PATCH... ' + (continentFound?.name ?? 'No data for Opti UI yet'),
					code,
					countries: []
				}
			},
			{ code },
			'store-only'
		);
		KQL_AllCountriesOfContinent.queryLoad({
			fetch,
			variables: { code }
		});
		return {};
	}
</script>

<script lang="ts">
</script>

<h1 class="text-xl">
	<a href="/continents">&laquo; Go back</a> | Continent:
	<b>{$KQL_AllCountriesOfContinent.data?.continent?.name}</b>
	({$KQL_AllCountriesOfContinent.data?.continent?.code})
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
				{#each $KQL_AllCountriesOfContinent.data?.continent?.countries || [] as country}
					<li class="my-3 text-lg">
						{country.code} - {country.name}
					</li>
				{:else}
					No countries
				{/each}
			</ul>
		{/if}
	</div>

	<KitQLInfo store={KQL_AllCountriesOfContinent} />

	<!-- Maybe 👆 is nicer than 👇 -->

	<!-- <pre class="whitespace-pre-wrap text-xs">{JSON.stringify(
			$KQL_AllCountriesOfContinent,
			null,
			' '
		)}</pre> -->
</div>
