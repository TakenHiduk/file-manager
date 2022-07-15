<script context="module" lang="ts">
	import type { Load, LoadOutput } from '@sveltejs/kit';

	export const load: Load = ({ params }): LoadOutput => {
		let parts: string | string[] = params.parts.trim();

		let doubleSlashRegex = /\/\//g;
		while (doubleSlashRegex.test(parts.trim())) {
			parts = parts.replace(doubleSlashRegex, '/');
		}
		parts = parts.split('/');

		return { props: { parts } };
	};
</script>

<script lang="ts">
	import { goto } from '$app/navigation';

	export let parts: string[];

	const switchFolder = async (index?: number) => {
		let newParts = [];

		if (index !== undefined) {
			newParts = parts.slice(0, index + 1);
		} else {
			newParts = [...parts, makeid(1)];
		}
		const path = newParts.join('/');
		const url = `/file-system/${path}`;

		await goto(url);
	};

	function makeid(length: number) {
		var result = '';
		var characters =
			'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
		var charactersLength = characters.length;
		for (var i = 0; i < length; i++) {
			result += characters.charAt(Math.floor(Math.random() * charactersLength));
		}
		return result;
	}
</script>

<style lang="scss">
	.breadcrums {
		display: flex;
		gap: 5px;
		align-items: center;
		justify-items: center;
	}
</style>

<div class="breadcrums">
	<button on:click|preventDefault={() => goto('/file-system')}>
		<span> Drives </span>
	</button>
	{#if parts.length > 0}
		<span> > </span>
	{/if}
	{#each parts as part, index}
		<button on:click|preventDefault={() => switchFolder(index)}>
			<span> {part} </span>
		</button>
		{#if index !== parts.length - 1}
			<span> > </span>
		{/if}
	{/each}
</div>

<button on:click|preventDefault={() => switchFolder()}>
	append random path
</button>
