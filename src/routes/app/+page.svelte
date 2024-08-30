<script lang="ts">
	import Calendar from '@event-calendar/core';
	import TimeGrid from '@event-calendar/time-grid';
	import ResourceTimeLine from '@event-calendar/resource-timeline';

	import type { PageData } from './$types';
	export let data: PageData;

	let plugins = [TimeGrid];
	let options = {
		view: 'timeGriMonth',
		events: [
			// your list of events
		]
	};
</script>

<Calendar {plugins} {options} />
