<script>
	import Separator from '$lib/components/ui/separator/separator.svelte';
	import '../app.css';
	import { ModeWatcher } from 'mode-watcher';
</script>

<ModeWatcher defaultMode="dark" />
<div class="content flex min-h-screen flex-row">
	<div class="flex h-full min-h-full w-full flex-col justify-between overflow-auto p-4">
		<div>
			<slot />
		</div>
		<div class="flex w-full flex-col gap-2 self-center font-medium pt-4">
			<div class="flex w-full flex-col gap-2 self-center font-medium">
				<Separator />
				<div class="flex flex-row gap-2 self-center">
					<a href="https://github.com/tzezar"
					class="text-primary font-medium underline underline-offset-4"
						>Sebastian "Tzezar" Drozd {new Date().getFullYear()}</a
					>
				</div>
			</div>
			
		</div>
		<div class="flex flex-row gap-2 self-center">
			<Separator />
		</div>
	</div>
</div>
