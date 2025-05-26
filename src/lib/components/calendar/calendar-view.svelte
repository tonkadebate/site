<script lang="ts">
    import { getLocalTimeZone, today } from "@internationalized/date";

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

    // Get current date
    let currentDate = $state(today(getLocalTimeZone()));

    // Convert events to dates that the calendar can understand and create a map
    let eventsByDate = $derived(() => {
        const eventMap = new Map();
        events.forEach(event => {
            if (!eventMap.has(event.date)) {
                eventMap.set(event.date, []);
            }
            eventMap.get(event.date).push(event);
        });
        console.log('Events by date:', eventMap); // Debug log
        return eventMap;
    });

    // Generate calendar days for the current month
    let calendarDays = $derived(() => {
        const year = currentDate.year;
        const month = currentDate.month;
        
        // First day of the month
        const firstDay = currentDate.set({ day: 1 });
        // Last day of the month  
        const lastDay = currentDate.set({ day: currentDate.set({ month: month + 1, day: 1 }).subtract({ days: 1 }).day });
        
        // Get the day of week for the first day (0 = Sunday, 1 = Monday, etc.)
        // Convert to Monday = 0, Tuesday = 1, etc.
        const startPaddingRaw = firstDay.toDate(getLocalTimeZone()).getDay();
        const startPadding = startPaddingRaw === 0 ? 6 : startPaddingRaw - 1;
        
        const days = [];
        
        // Add empty cells for days before the month starts
        for (let i = 0; i < startPadding; i++) {
            days.push(null);
        }
        
        // Add all days of the month
        for (let day = 1; day <= lastDay.day; day++) {
            const date = currentDate.set({ day });
            const dateString = date.toString();
            console.log('Calendar date string:', dateString); // Debug log
            days.push({
                date,
                dateString,
                day: day,
                isToday: date.compare(today(getLocalTimeZone())) === 0
            });
        }
        
        return days;
    });

    // Navigate months
    function previousMonth() {
        currentDate = currentDate.subtract({ months: 1 });
    }

    function nextMonth() {
        currentDate = currentDate.add({ months: 1 });
    }

    // Get month/year display
    let monthYearDisplay = $derived(
        currentDate.toDate(getLocalTimeZone()).toLocaleDateString('default', { 
            month: 'long', 
            year: 'numeric' 
        })
    );

    // Debug: log events when they change
    $effect(() => {
        console.log('Events received:', events);
    });
</script>

<section id="calendar" class="space-y-4 md:mx-8">
    <h2 class="text-2xl font-bold">Calendar</h2>
    
    
    <div class="w-full mx-auto border rounded-lg">
        <!-- Calendar Header -->
        <div class="flex items-center justify-between p-4 border-b">
            <button 
                onclick={previousMonth}
                class="p-2 hover:bg-gray-100 rounded"
            >
                ←
            </button>
            <h3 class="text-xl font-semibold">{monthYearDisplay}</h3>
            <button 
                onclick={nextMonth}
                class="p-2 hover:bg-gray-100 rounded"
            >
                →
            </button>
        </div>

        <!-- Day Headers - Monday first -->
        <div class="grid grid-cols-7 border-b">
            {#each ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'] as dayName}
                <div class="p-2 text-center font-medium text-gray-600 border-r last:border-r-0">
                    {dayName}
                </div>
            {/each}
        </div>

        <!-- Calendar Grid -->
        <div class="grid grid-cols-7">
            {#each calendarDays() as day}
                <div class="min-h-20 border-r border-b last:border-r-0 p-2">
                    {#if day}
                        <div class="h-full">
                            <!-- Day Number -->
                            <div class="flex justify-between items-start mb-1">
                                <span class="font-medium {day.isToday ? 'bg-primary text-primary-foreground rounded-full w-6 h-6 flex items-center justify-center text-sm' : ''}">
                                    {day.day}
                                </span>
                            </div>
                            
                            <!-- Events for this day -->
                            {#if eventsByDate().has(day.dateString)}
                                <div class="space-y-1">
                                    {#each eventsByDate().get(day.dateString).slice(0, 3) as event}
                                        <div class="text-xs bg-primary/20 text-primary px-2 py-1 rounded truncate">
                                            {event.title}
                                        </div>
                                    {/each}
                                    {#if eventsByDate().get(day.dateString).length > 3}
                                        <div class="text-xs text-muted-foreground">
                                            +{eventsByDate().get(day.dateString).length - 3} more
                                        </div>
                                    {/if}
                                </div>
                            {/if}
                        </div>
                    {/if}
                </div>
            {/each}
        </div>
    </div>
</section>