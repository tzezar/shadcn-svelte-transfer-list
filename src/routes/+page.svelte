<script lang="ts">
	import TransferList from '$lib/components/ui/transfer-list/transfer-list.svelte';
	import * as Card from '$lib/components/ui/card';
	import ScrollArea from '$lib/components/ui/scroll-area/scroll-area.svelte';
	import Highlight from 'svelte-highlight';
	import typescript from 'svelte-highlight/languages/typescript';
	import atomOneDark from 'svelte-highlight/styles/atom-one-dark';
	import { mode } from 'mode-watcher';
	import logoBlack from '$lib/assets/tzezar-logo-black.png';
	import logoWhite from '$lib/assets/tzezar-logo-white.png';

	let source: { id: number; name: string }[] = [
		{ id: 1, name: 'Kingdom of Heaven 2005' },
		{ id: 2, name: 'The Lord of the Rings: The Fellowship of the Ring 2001' },
		{ id: 3, name: 'The Shawshank Redemption 1994' },
		{ id: 4, name: 'Forrest Gump 1994' },
		{ id: 5, name: 'Se7en 1995' }
	];
	let target: typeof source = [];

	function onChange(event: CustomEvent) {
		source = event.detail.source;
		target = event.detail.target;
	}

	let code1 = `<script lang='ts'>
    import TransferList from '$lib/components/ui/transfer-list/transfer-list.svelte';
    
    // in current version shape of source object should be { id: number; name: string }, you can change it for your needs in the component
    let source: { id: number; name: string }[] = [
        { id: 1, name: 'Kingdom of Heaven 2005' },
        { id: 2, name: 'The Lord of the Rings: The Fellowship of the Ring 2001' },
        { id: 3, name: 'The Shawshank Redemption 1994' },
        { id: 4, name: 'Forrest Gump 1994' },
        { id: 5, name: 'Se7en 1995' }
    ];
    let target: typeof source = [];

    function onChange(event: CustomEvent) {
        source = event.detail.source;
        target = event.detail.target;
    }
<\/script> 

<TransferList
    bind:source
    bind:target
    sourceHeader="Movies"
    targetHeader="Favourite Movies"
    filterBy="name"
    on:change={onChange}
/>
`;
</script>

<svelte:head>
	{@html atomOneDark}
</svelte:head>
<div class="flex flex-col gap-4">
	<div class='flex flex-row gap-4'>
		{#if $mode == 'dark'}
		<img src={logoWhite} alt="" srcset="" class="h-[64px]" />
		{:else}
		<img src={logoBlack} alt="" srcset="" class="h-[64px]" />
		<!-- else content here -->
		{/if}
		<h1 class="scroll-m-20 text-4xl font-extrabold tracking-tight lg:text-5xl">

			shadcn-svelte transfer-list
		</h1>
	</div>
	<h2
		class="mt-10 scroll-m-20 border-b pb-2 text-3xl font-semibold tracking-tight transition-colors first:mt-0"
	>
		Showcase
	</h2>
	<TransferList
		bind:source
		bind:target
		sourceHeader="Movies"
		targetHeader="Favourite Movies"
		filterBy="name"
		on:change={onChange}
	/>
	<div class="flex flex-wrap gap-4 md:flex-nowrap">
		<Card.Root class="w-full md:w-1/2">
			<Card.Header>
				<Card.Title>Movies</Card.Title>
			</Card.Header>
			<Card.Content>
				<ScrollArea>
					<code>
						{JSON.stringify(source, null, 2)}
					</code>
				</ScrollArea>
			</Card.Content>
		</Card.Root>
		<Card.Root class="w-full md:w-1/2">
			<Card.Header>
				<Card.Title>Favourite Movies</Card.Title>
			</Card.Header>
			<Card.Content>
				<code class="">
					{JSON.stringify(target, null, 2)}
				</code>
			</Card.Content>
		</Card.Root>
	</div>
	<div class="flex flex-col gap-4">
		<h2
			class="mt-10 scroll-m-20 border-b pb-2 text-3xl font-semibold tracking-tight transition-colors first:mt-0"
		>
			Usage
		</h2>

		<p>
			1. Copy component from <a
				class="font-medium text-primary underline underline-offset-4"
				href="https://github.com/tzezar/shadcn-svelte-transfer-list/blob/main/src/lib/components/ui/transfer-list/transfer-list.svelte"
				>GITHUB REPOSITORY</a
			> and paste it into your components folder.
		</p>
		<p>2. Use the component in your Svelte file as shown below:</p>
		<Highlight language={typescript} code={code1} />
		<p class="text-sm text-muted-foreground">
			Remember to add shadcn-svelte Button, Checkbox, ScrollArea, Input components.
		</p>
	</div>
	<p class="pt-4">
		Thanks to <a
			class="font-medium text-primary underline underline-offset-4"
			href="https://khromov.se/">khromov</a
		> for creating the base svelte version.
	</p>
	<p class="pb-6 italic">
		If you find the project interesting feel free to give a star on <a
		class="font-medium text-primary underline underline-offset-4"
			href="https://github.com/tzezar/shadcn-svelte-transfer-list">github</a
		>. Thanks! ❤️
	</p>
</div>
