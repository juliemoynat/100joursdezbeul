<script lang="ts">
	import { ACTION_LABEL, ACTION_SCORE, TARGET_LABEL, TARGET_MULTIPLIER } from '$lib/constants';
	import type { ActionEvent } from '$lib/types';
	import { getPointsDisplay, humanizeLink } from '$lib/utils';

	export let event: ActionEvent;
	export let hideDate = false;

	const liens = event.liens.filter((lien) => !!lien).map((lien) => humanizeLink(lien));

	function getDateLabel(datestring: string) {
		return new Date(datestring).toLocaleDateString('fr-FR', { dateStyle: 'short' }).slice(0, 5);
	}
</script>

<li class="mb-8 flex flex-row">
	<div class="w-20 shrink-0">
		{#if !hideDate}<strong>{getDateLabel(event.date)}</strong>{/if}
		<span class="font-bold text-[#dd0220]">{@html getPointsDisplay(event.score)}</span><br />
	</div>
	<div>
		<span class="font-bold">{event.ville}&nbsp;:</span>
		{event.description}
		<div class="mt-1 flex flex-wrap gap-1">
			{#each event.actions as action}
				<div class="tag action">
					{ACTION_LABEL[action]}&nbsp;: {@html getPointsDisplay(ACTION_SCORE[action])}
				</div>
			{/each}
		</div>
		<div class="mt-1 flex flex-wrap gap-1">
			{#each event.cibles as target}
				<div class="tag target">
					{TARGET_LABEL[target.role.code]}&nbsp;: x{TARGET_MULTIPLIER[target.role.code]}
				</div>
			{/each}
		</div>
		<div class="mt-1 flex flex-wrap gap-1">
			{#each liens as link}
				<div class="tag target">
					<a href={link.url}>{link.text}</a>
				</div>
			{/each}
		</div>
	</div>
</li>

<style lang="postcss">
	.tag {
		@apply whitespace-nowrap py-0.5 pr-2 text-xs font-bold;
	}

	.action {
		@apply text-slate-700;
	}

	.target {
		@apply text-slate-700;
	}
</style>
