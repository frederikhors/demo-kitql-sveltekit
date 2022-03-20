<script lang="ts">
	import "$lib/style.postcss";
</script>

<div class="container mx-auto mt-8">
	<div class="text-center text-lg">
		<a href="/">Home</a> | <a href="/continents">Continents</a>
	</div>

	<div class="mt-8">
		<slot />
	</div>
</div>
