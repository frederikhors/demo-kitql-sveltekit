<script context="module" lang="ts">
	import { KQL_AllContinents } from '$lib/kitql/generated/stores';

	export async function load({ url, fetch, session, stuff }) {
		await KQL_AllContinents.query({ fetch });
		return {};
	}
</script>

<script lang="ts">
	// import { browser } from '$app/env';

	// If you want to use it ONLY client mode, but if you want NETWORK or SSR, put it in the module
	// $: browser && KQL_AllContinents.query();
</script>

<h1 class="text-xl">Continents</h1>

<div class="grid grid-cols-3 mt-4">
	<div class="col-span-2">
		{#if $KQL_AllContinents.isFetching}
			Loading...
		{:else if $KQL_AllContinents.errors}
			{#each $KQL_AllContinents.errors as error}
				{error}
			{/each}
		{:else}
			<ul>
				{#each $KQL_AllContinents.data?.continents || [] as continent}
					<li class="my-3 text-lg">
						<a href="/continents/{continent.code}">
							{continent.code} - {continent.name}
						</a>
					</li>
				{/each}
			</ul>
		{/if}
	</div>

	<pre class="whitespace-pre-wrap text-xs">{JSON.stringify($KQL_AllContinents, null, ' ')}</pre>
</div>
