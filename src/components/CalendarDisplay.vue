<template>
    <div class="calendar">
        <!-- Glava koledarja za velik zaslon -->
        <div class="day header hidden show-lg bg-green">Ponedeljek</div>
        <div class="day header hidden show-lg bg-blue">Torek</div>
        <div class="day header hidden show-lg bg-red">Sreda</div>
        <div class="day header hidden show-lg bg-orange">Četrtek</div>
        <div class="day header hidden show-lg bg-green">Petek</div>
        <div class="day header hidden show-lg bg-blue">Sobota</div>
        <div class="day header hidden show-lg bg-red">Nedelja</div>

        <!-- Glava koledarja za manjši zaslon -->
        <div class="day header hidden-lg bg-green">Pon</div>
        <div class="day header hidden-lg bg-blue">Tor</div>
        <div class="day header hidden-lg bg-red">Sre</div>
        <div class="day header hidden-lg bg-orange">Čet</div>
        <div class="day header hidden-lg bg-green">Pet</div>
        <div class="day header hidden-lg bg-blue">Sob</div>
        <div class="day header hidden-lg bg-red">Ned</div>

        <!-- Mreža dni z oznakami -->
        <div v-for="day, i in calendarDays" class="day" :class="{'holiday': day?.isHoliday, 'sunday': (i + 1) % 7 == 0}">
            <div class="day-number">{{ day?.day }}</div>
            <div v-if="day?.isHoliday" class="holiday-text hidden show-lg">Praznik</div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

type CalendarDate = {
    day: Number,
    isHoliday: boolean
}

const props = defineProps({
    year: Number,
    month: Number,
    holidays: Array<Number> || null
})

// Seznam dni v mesecu za prikaz na koledarju
const calendarDays = computed(() => {

    if(typeof props.year === 'undefined' || typeof props.month === 'undefined') {
        return []
    }

    // Dolžina meseca
    const monthLength = new Date(props.year, props.month + 1, 0).getDate()

    // Prvi dan v mesecu (oznaka zamaknjena, da je ponedeljek na prvem mestu)
    const firstDay = (new Date(props.year, props.month, 1).getDay() + 6) % 7

    // Seznam dni v mesecu inicializiran s praznimi vrednostmi
    const days: (CalendarDate | null)[]= new Array(42).fill(null)

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