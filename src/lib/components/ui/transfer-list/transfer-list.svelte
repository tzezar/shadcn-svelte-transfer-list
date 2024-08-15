<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import { ScrollArea } from '$lib/components/ui/scroll-area/index.js';
	import Checkbox from '$lib/components/ui/checkbox/checkbox.svelte';
	import Input from '$lib/components/ui/input/input.svelte';
	import Button from '$lib/components/ui/button/button.svelte';

	type Item = {
		id: number;
		[key: string]: unknown;
	};

	// Main Picklist props
	export let source: Item[] = [];
	export let target: Item[] = [];
	export let sourceHeader = '';
	export let targetHeader = '';
	export let filterBy = '';

	// Internal state
	let sourceSelection: Item[] = [];
	let targetSelection: Item[] = [];
	let sourceFilterValue = '';
	let targetFilterValue = '';

	const dispatch = createEventDispatcher();

	// Filtering logic
	$: filteredSource =
		filterBy && sourceFilterValue
			? source.filter((item) =>
					String(item[filterBy]).toLowerCase().includes(sourceFilterValue.toLowerCase())
				)
			: source;

	$: filteredTarget =
		filterBy && targetFilterValue
			? target.filter((item) =>
					String(item[filterBy]).toLowerCase().includes(targetFilterValue.toLowerCase())
				)
			: target;

	// Event handlers
	function onTransfer(newSource: Item[], newTarget: Item[]) {
		source = newSource;
		target = newTarget;
		sourceSelection = [];
		targetSelection = [];
		dispatchChange();
	}

	function dispatchChange() {
		dispatch('change', { source, target });
	}

	// Selection logic
	function onSelectionChange(list: any, selection: Item[], item: Item) {
		const index = selection.indexOf(item);
		if (index > -1) {
			return selection.filter((_, i) => i !== index);
		} else {
			return [...selection, item];
		}
	}

	function onSourceSelectionChange(item: Item) {
		sourceSelection = onSelectionChange(source, sourceSelection, item);
	}

	function onTargetSelectionChange(item: Item) {
		targetSelection = onSelectionChange(target, targetSelection, item);
	}

	// Transfer logic
	function moveToTarget() {
		const newSource = source.filter((item) => !sourceSelection.includes(item));
		const newTarget = [...target, ...sourceSelection];
		onTransfer(newSource, newTarget);
	}

	function moveToSource() {
		const newTarget = target.filter((item) => !targetSelection.includes(item));
		const newSource = [...source, ...targetSelection];
		onTransfer(newSource, newTarget);
	}

	function moveAllToTarget() {
		onTransfer([], [...target, ...source]);
	}

	function moveAllToSource() {
		onTransfer([...source, ...target], []);
	}
</script>

<div class="flex flex-col gap-4 md:flex-row">
	<div class="w-full">
		{#if sourceHeader}
			<div class="pb-2">{sourceHeader}</div>
		{/if}

		{#if filterBy}
			<div class="flex flex-row">
				<Input
					type="text"
					class="rounded-b-none"
					placeholder="Filter"
					bind:value={sourceFilterValue}
				/>
				<div class="flex flex-row">
					<Button class="w-10 rounded-none" on:click={moveToTarget}>&gt;</Button>
					<Button class="w-10 rounded-none rounded-tr-md" on:click={moveAllToTarget}
						>&gt;&gt;</Button
					>
				</div>
			</div>
		{/if}

		<ul class="flex h-[140px] flex-col rounded-b-md border-[1px] border-solid border-input p-2 ">
			<ScrollArea>
				{#each filteredSource as item (item.id)}
					<!-- svelte-ignore a11y-click-events-have-key-events -->
					<!-- svelte-ignore a11y-no-static-element-interactions -->
					<div
						class="flex flex-row items-center gap-x-2 rounded-md px-1 py-1 hover:bg-secondary mr-4"
						on:click={() => onSourceSelectionChange(item)}
					>
						<Checkbox class='self-start mt-1' checked={sourceSelection.includes(item)} />
						<li class="w-full">
							{item[filterBy]}
						</li>
					</div>
				{/each}
			</ScrollArea>
		</ul>
	</div>
	<div class="w-full">
		{#if targetHeader}
			<div class="pb-2">{targetHeader}</div>
		{/if}

		{#if filterBy}
			<div class="flex flex-row">
				<Button class="w-10 rounded-none rounded-tl-md" on:click={moveAllToSource}>&lt;&lt;</Button>
				<Button class="w-10 rounded-none" on:click={moveToSource}>&lt;</Button>
				<Input
					type="text"
					class="p-picklist-filter"
					placeholder="Filter"
					bind:value={targetFilterValue}
				/>
			</div>
		{/if}

		<ul class="flex h-[140px] flex-col rounded-b-md border-[1px] border-solid border-input p-2">
			<ScrollArea>
				{#each filteredTarget as item (item.id)}
					<!-- svelte-ignore a11y-click-events-have-key-events -->
					<!-- svelte-ignore a11y-no-static-element-interactions -->
					<div
						class="flex flex-row items-center gap-x-2 rounded-md px-1 py-1 hover:bg-secondary pr-4"
						on:click={() => onTargetSelectionChange(item)}
					>
						<Checkbox class='self-start mt-1' checked={targetSelection.includes(item)} />
						<li class="w-full">
							{item[filterBy]}
						</li>
					</div>
				{/each}
			</ScrollArea>
		</ul>
	</div>
</div>
