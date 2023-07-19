<script lang="ts">
	import { modalStore, ProgressRadial } from '@skeletonlabs/skeleton';
	import { confettiAction } from 'svelte-legos';
	import { chatStore, settingsStore } from '$misc/stores';
	import { canSuggestTitle, suggestChatTitle, track } from '$misc/shared';

	let slug = $modalStore[0].meta?.slug || '';
	let isLoading = false;

	let title = $chatStore[slug].title;

	$: showAiSuggestOptions = $settingsStore.openAiApiKey && canSuggestTitle($chatStore[slug]);

	async function handleSuggestTitle() {
		if (!$settingsStore.openAiApiKey) {
			return;
		}
		isLoading = true;
		title = await suggestChatTitle($chatStore[slug], $settingsStore.openAiApiKey);
		isLoading = false;
		track('suggestTitleManually');
	}

	function handleSave() {
		chatStore.updateChat(slug, { title });

		if ($modalStore[0].response) {
			$modalStore[0].response(true);
		}
		modalStore.close();
	}
</script>

<div class="card variant-filled-surface-700 p-8">
	<h3 class="mb-4">Subject</h3>

	<form class="flex flex-col space-y-4">
		<label class="label">
			<span class="inline-block w-40">Set which subject your working in.</span>
			<input type="text" class="input" bind:value={title} />
		</label>

		{#if showAiSuggestOptions}
			<span class="self-center">OR:</span>

			<button
				class="btn variant variant-filled-secondary"
				disabled={isLoading}
				use:confettiAction={{ type: 'school-pride' }}
				on:click={handleSuggestTitle}
			>
				{#if !isLoading}
					Let AI suggest study materials for this subject.
				{:else}
					<ProgressRadial
						class="w-6"
						stroke={120}
						meter="stroke-tertiary-500"
						track="stroke-tertiary-500/30"
					/>
				{/if}
			</button>

			<span class="text-xs text-slate-400">
				This suggestion will help in overall context and specificity of the responses.
			</span>
		{/if}

		<div class="flex flex-col md:flex-row space-y-4 md:space-y-0 justify-between items-center">
			{#if showAiSuggestOptions}
				<label class="flex items-center space-x-2">
					<input
						class="checkbox"
						type="checkbox"
						bind:checked={$settingsStore.useTitleSuggestions}
					/>
					<p>Set a general title all subjects</p>
				</label>
			{/if}

			<button class="btn variant-filled-primary max-w-[100px] self-end" on:click={handleSave}>
				Save
			</button>
		</div>
	</form>
</div>
