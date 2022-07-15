<script lang="ts">
	import { goto } from '$app/navigation';

	const drives: string[] = ['C', 'acadia01', 'pr-acclaim-dl06'];
</script>

<h1>File System</h1>

<h2>Listing drives...</h2>

{#each drives as drive}
	<button on:click|preventDefault={() => goto(`/file-system/${drive}`)}>
		{drive}
	</button>
{/each}
