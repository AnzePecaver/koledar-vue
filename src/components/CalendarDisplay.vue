<template>
    <div class="calendar">
        <!-- Glava koledarja za velik zaslon -->
        <div class="day header hidden show-lg ">Ponedeljek</div>
        <div class="day header hidden show-lg ">Torek</div>
        <div class="day header hidden show-lg ">Sreda</div>
        <div class="day header hidden show-lg ">Četrtek</div>
        <div class="day header hidden show-lg ">Petek</div>
        <div class="day header hidden show-lg ">Sobota</div>
        <div class="day header hidden show-lg ">Nedelja</div>

        <!-- Glava koledarja za manjši zaslon -->
        <div class="day header hidden-lg ">Pon</div>
        <div class="day header hidden-lg ">Tor</div>
        <div class="day header hidden-lg ">Sre</div>
        <div class="day header hidden-lg ">Čet</div>
        <div class="day header hidden-lg ">Pet</div>
        <div class="day header hidden-lg ">Sob</div>
        <div class="day header hidden-lg ">Ned</div>

        <!-- Mreža dni z oznakami -->
        <div v-for="(day, i) in calendarDays" :key="i" class="day" :class="{'holiday': day?.isHoliday, 'sunday': (i + 1) % 7 == 0}">
            <div class="day-number">{{ day ? day.day : "" }}</div>
            <div v-if="day?.isHoliday" class="holiday-text hidden show-lg">Praznik</div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

type CalendarDate = {
    day: number,
    isHoliday: boolean
}

// Pridobljene vrednosti leto, mesec (0 - 11) in seznam aktualnih praznikov za izbrani mesec in leto
const props = defineProps<{
    year?: number,
    month?: number,
    holidays?: number[]
}>()

// Seznam dni v mesecu za prikaz na koledarju
const calendarDays = computed(() => {

    // Seznam dni v mesecu inicializiran s praznimi vrednostmi
    const days: (CalendarDate | null)[]= new Array(42).fill(null)

    if(typeof props.year === 'undefined' || typeof props.month === 'undefined') {
        return days
    }

    // Dolžina meseca
    const monthLength = new Date(props.year, props.month + 1, 0).getDate()

    // Prvi dan v mesecu (oznaka zamaknjena, da je ponedeljek na prvem mestu)
    const firstDay = (new Date(props.year, props.month, 1).getDay() + 6) % 7

    // Seznam napolnjen z vrednostjo dneva in oznako, ali je praznik
    for (let i = firstDay; i < monthLength + firstDay; i++) {
        let day = i - firstDay + 1

        days[i] = {
            day: day,
            isHoliday: props.holidays ? props.holidays.includes(day) : false
        }
    }

    return days
})

</script>

<style scoped>

.calendar {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 0.25rem;
    filter: drop-shadow(0 0 0.1rem #e0e0e8);
}

.day {
    display: inline-block;
    color: #303030;
    font-weight: bold;
    font-size: 1rem;
    padding: 1rem 1rem 4.5rem;
    border-radius: 0.25rem;
    
    background-color: #fafaff;
}

.day-number {
    height: 1.75rem;
    width: 1.75rem;
    border-radius: 50%;
    display: grid;
    place-items: center;
}

.holiday .day-number {
    background-color: #cacafd;
}

.day.header {
    padding: 1rem;
    text-align: center;
    text-transform: uppercase;
    margin-bottom: 0.25rem;
    background: #fefeff;
}

.day.holiday {
    color: #08080f;
    padding: 1rem;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.day.sunday {
    color: #ff3c00;
}

.holiday-text {
    color: #9898dd;
    font-size: 0.875rem;
    padding-top: 2.125rem;
    font-weight: normal;
    text-align: right;
}

.hidden {
    display: none;
}

@media screen and (max-width: 900px) {
    .day {
        padding: 0.5rem;
        text-align: center;
    }
    .day.header {
        font-size: 0.75rem;
        padding: 0.5rem;
    }
    .day.holiday {
        display: inline-block;
        padding: 0.5rem;
    }
}

@media screen and (min-width: 901px) {
    .hidden-lg {
        display: none;
    }
    .show-lg {
        display: inline-block;
    }
}
</style>