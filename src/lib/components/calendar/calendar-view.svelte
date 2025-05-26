<script lang="ts">
	import { Calendar } from "$lib/components/ui/calendar";
	import { addMonths, startOfMonth } from "@internationalized/date";
	import { getLocalTimeZone, today } from "@internationalized/date";

	// Get current date and next two months
	let currentDate = $state(today(getLocalTimeZone()));
	let nextMonth = $state(addMonths(currentDate, 1));
	let nextTwoMonths = $state(addMonths(currentDate, 2));

	// Mock events data (would come from YAML in real app)
	const events = [
		{ date: "2024-03-15", type: "competition" },
		{ date: "2024-03-20", type: "practice" },
		{ date: "2024-03-25", type: "practice" },
		{ date: "2024-04-05", type: "competition" },
		{ date: "2024-04-15", type: "practice" },
		{ date: "2024-05-01", type: "competition" },
	];
</script>

<section id="calendar" class="space-y-4">
	<h2 class="text-2xl font-bold">Calendar</h2>
	<div class="grid gap-4 md:grid-cols-2 lg:grid-cols-3">
		<div>
			<h3 class="mb-2 text-center font-semibold">March 2024</h3>
			<Calendar type="single" value={currentDate} class="rounded-md border" />
		</div>
		<div>
			<h3 class="mb-2 text-center font-semibold">April 2024</h3>
			<Calendar type="single" value={nextMonth} class="rounded-md border" />
		</div>
		<div>
			<h3 class="mb-2 text-center font-semibold">May 2024</h3>
			<Calendar type="single" value={nextTwoMonths} class="rounded-md border" />
		</div>
	</div>
</section>
