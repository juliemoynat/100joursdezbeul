<script lang="ts">
	import {
		filterEventsForDate,
		filterEventsForDepartment,
		getDepartmentName,
		getPointsDisplay,
		getScoreForEvents
	} from '$lib/utils';
	import Event from '$lib/event.svelte';
	import { DEPARTMENTS } from '$lib/constants';

	export let data;

	const date = new Date(data.date);
	const formattedDate = date?.toLocaleDateString('fr', { dateStyle: 'medium' });
	const events = filterEventsForDate(date, data.actionEvents);

	function getScore(departmentCode: string): number {
		return getScoreForEvents(filterEventsForDepartment(departmentCode, events));
	}
</script>

<svelte:head>
	<title>Zbeul du jour | 100 jours de zbeul</title>
</svelte:head>

<h2 class="zbeul mb-16 mt-16 text-4xl">Le zbeul du {formattedDate}</h2>

<div class="mx-auto mb-24 mt-10 max-w-2xl text-xl">
	{#if events.length}
		<ul>
			{#each DEPARTMENTS.map((d) => d.code) as deptCode}
				{@const deptEvents = filterEventsForDepartment(deptCode, events)}
				{#if deptEvents.length}
					<h3 class="zbeul mb-6 mt-10 text-2xl">
						{getDepartmentName(deptCode)}&nbsp;: +{@html getPointsDisplay(getScore(deptCode))}
					</h3>
					{#each deptEvents as event}
						<Event {event} hideDate />
					{/each}
				{/if}
			{/each}
		</ul>
	{:else}
		Aucun événement pris en compte ce jour-là.
	{/if}
</div>
