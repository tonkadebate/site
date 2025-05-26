<script lang="ts">
    // Import components
    import Nav from "$lib/components/nav/nav.svelte";
    import Hero from "$lib/components/hero/hero.svelte";
    import UpcomingEvents from "$lib/components/events/upcoming-events.svelte";
    import CalendarView from "$lib/components/calendar/calendar-view.svelte";
    import Footer from "$lib/components/footer/footer.svelte";
    import { Button } from "$lib/components/ui/button";
    import { ExternalLinkIcon } from "lucide-svelte";

    // Import YAML loader (e.g., yaml or js-yaml)
    import yaml from "js-yaml";
    import { onMount } from "svelte";

    // Store for events loaded from YAML using Svelte 5 runes
    let events = $state([]);

    // Load events from a YAML file in the static directory
    onMount(async () => {
        const res = await fetch("/events.yaml");
        const text = await res.text();
        const data = yaml.load(text);
        events = data.events ?? [];
    });
</script>

<!--<Nav />-->

<!-- Quick Links Section -->

<main class="">
    <div class="px-4 py-3 w-fit mx-auto pb-12 pt-4">
        <span class="text-sm font-medium text-gray-600 mr-2 pb-2">Links:</span>
        <div class="gap-4 sm:justify-start w-fit">
            <Button
                variant="outline"
                size="sm"
                href="https://www.tabroom.com"
                target="_blank"
                rel="noopener noreferrer"
                class="text-sm"
            >
                Tabroom
                <ExternalLinkIcon class="w-3 h-3 ml-1" />
            </Button>
            <Button
                variant="outline"
                size="sm"
                href="https://drive.google.com"
                target="_blank"
                rel="noopener noreferrer"
                class="text-sm"
            >
                Google Drive
                <ExternalLinkIcon class="w-3 h-3 ml-1" />
            </Button>
            <Button
                variant="outline"
                size="sm"
                href="https://groupme.com"
                target="_blank"
                rel="noopener noreferrer"
                class="text-sm"
            >
                GroupMe
                <ExternalLinkIcon class="w-3 h-3 ml-1" />
            </Button>
            <Button
                variant="outline"
                size="sm"
                href="https://instagram.com"
                target="_blank"
                rel="noopener noreferrer"
                class="text-sm"
            >
                Instagram
                <ExternalLinkIcon class="w-3 h-3 ml-1" />
            </Button>
        </div>
    </div>
    {#if events.length}
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <UpcomingEvents {events} />
        <div class="md:col-span-2">
        <CalendarView {events} />
        </div>
    </div>
    {:else}
        <p class="text-center text-muted-foreground mx-auto">Loading events...</p>
    {/if}
</main>