<script lang="ts">
	import { CalendarDaysIcon } from "lucide-svelte";
	import { Button } from "$lib/components/ui/button";
</script>

<header class="sticky top-0 z-50 w-full border-b bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60">
	<div class="container flex h-16 items-center px-4">
		<a href="/" class="flex items-center gap-2 font-bold">
			<CalendarDaysIcon />
			<span>Debate Team</span>
		</a>
		<nav class="ml-auto flex gap-4">
			<Button variant="ghost">Schedule</Button>
			<Button variant="ghost">Team</Button>
			<Button variant="ghost">About</Button>
		</nav>
	</div>
</header>
