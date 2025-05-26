<script lang="ts">
	import { CalendarPlusIcon } from "lucide-svelte";
	import { Button } from "$lib/components/ui/button";
	import { Separator } from "$lib/components/ui/separator";
</script>

<section id="hero" class="text-center">
	<h1 class="text-4xl font-bold">Debate Team Calendar</h1>
	<p class="mt-2 text-muted-foreground">Stay updated with our debate schedule and competitions</p>
	<div class="mt-6 flex justify-center gap-4">
		<Button>
			<CalendarPlusIcon class="mr-2 h-4 w-4" />
			Add to Google Calendar
		</Button>
		<Separator orientation="vertical" />
		<Button variant="outline">Download iCal</Button>
	</div>
</section>
