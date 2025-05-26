<script lang="ts">
    import { MapPinIcon, ClockIcon } from "lucide-svelte";

    // Accept events as a prop using Svelte 5 syntax
    let { events = [] }: { 
        events: Array<{
            title: string;
            date: string;
            time?: string;
            location?: string;
            type?: string;
        }>
    } = $props();
</script>

<section id="upcoming" class="space-y-4 w-fit mx-auto">
    <h2 class="text-2xl font-bold">Upcoming</h2>
    {#if events && events.length}
        <ul class="space-y-4">
            {#each events.slice(0, 3) as event}
                <li class="border-l-4 border-primary pl-4 py-2">
                    <h3 class="font-semibold text-lg">{event.title}</h3>
                    <div class="flex gap-4 mt-1 space-y-1 text-sm text-muted-foreground">
                        {#if event.date || event.time}
                            <div class="flex items-center gap-2">
                                <ClockIcon class="h-4 w-4" />
                                <span>
                                    {event.date}
                                    {#if event.time} at {event.time}{/if}
                                </span>
                            </div>
                        {/if}
                        {#if event.location}
                            <div class="flex items-center gap-2 pb-1">
                                <MapPinIcon class="h-4 w-4" />
                                <span>{event.location}</span>
                            </div>
                        {/if}
                    </div>
                </li>
            {/each}
        </ul>
    {:else}
        <p class="text-muted-foreground text-center">No upcoming events.</p>
    {/if}
</section>