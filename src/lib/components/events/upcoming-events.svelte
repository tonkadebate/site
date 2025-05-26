<script lang="ts">
	import { Card, CardContent, CardHeader, CardTitle } from "$lib/components/ui/card";
	import { MapPinIcon, ClockIcon } from "lucide-svelte";

	// Mock upcoming events data (would come from YAML in real app)
	const upcomingEvents = [
		{
			title: "Regional Debate Championship",
			date: "March 15, 2024",
			time: "9:00 AM",
			location: "Main Auditorium",
		},
		{
			title: "Practice Session",
			date: "March 20, 2024",
			time: "3:30 PM",
			location: "Room 204",
		},
		{
			title: "State Finals Preparation",
			date: "March 25, 2024",
			time: "4:00 PM",
			location: "Library Conference Room",
		},
	];
</script>

<section id="upcoming" class="space-y-4">
	<h2 class="text-2xl font-bold">Upcoming Events</h2>
	<div class="grid gap-4 sm:grid-cols-2 lg:grid-cols-3">
		{#each upcomingEvents as event}
			<Card>
				<CardHeader>
					<CardTitle class="text-lg">{event.title}</CardTitle>
				</CardHeader>
				<CardContent>
					<div class="space-y-2 text-sm">
						<div class="flex items-center gap-2">
							<ClockIcon class="h-4 w-4" />
							<span>{event.date} at {event.time}</span>
						</div>
						<div class="flex items-center gap-2">
							<MapPinIcon class="h-4 w-4" />
							<span>{event.location}</span>
						</div>
					</div>
				</CardContent>
			</Card>
		{/each}
	</div>
</section>
