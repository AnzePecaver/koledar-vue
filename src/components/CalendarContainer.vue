<template>
    <div class="calendar-container">
        <div class="error" v-if="holidayError">{{ holidayError }}</div>
        <div class="calendar-head">
            <h1>Moj Koledar</h1>
            <!-- Nadzor koledarja -->
            <div class="calendar-controls">
                <div class="month-year-selector">
                    <!-- Izbira meseca -->
                    <select name="month" id="monthInput" v-model="month">
                        <option :value="0">Januar</option>
                        <option :value="1">Februar</option>
                        <option :value="2">Marec</option>
                        <option :value="3">April</option>
                        <option :value="4">Maj</option>
                        <option :value="5">Junij</option>
                        <option :value="6">Julij</option>
                        <option :value="7">Avgust</option>
                        <option :value="8">September</option>
                        <option :value="9">Oktober</option>
                        <option :value="10">November</option>
                        <option :value="11">December</option>
                    </select>
                    <!-- Vpis leta -->
                    <input type="number" id="yearInput" name="year" v-model="year">
                </div>
                <!-- Vpis datuma -->
                <input type="date" id="dateInput" name="date" v-model="date">
            </div>
        </div>
        
        <!-- Prikaz koledarja -->
        <CalendarDisplay :year="year" :month="month" :holidays="actualHolidays"></CalendarDisplay>
    </div>
</template>

<script setup lang="ts">

import { ref, watch, computed } from 'vue';
import CalendarDisplay from './CalendarDisplay.vue';

const currentDate = new Date()

// Trenutno izbrano leto in mesec (0 - 11)
const year = ref(currentDate.getFullYear())
const month = ref(currentDate.getMonth())

// Trenutno izbrani datum
const date = ref(`${currentDate.getFullYear()}-${String(currentDate.getMonth() + 1).padStart(2, "0")}-${String(currentDate.getDate()).padStart(2, "0")}`)

// Posodabljanje trenutno izbranega leta in meseca glede na izbrani datum
watch(date, (newDate) => {
    const newDateParts = newDate.split('-')
    const newYear = parseInt(newDateParts[0])
    const newMonth = parseInt(newDateParts[1])

    if(!isNaN(newYear)) {
        year.value = newYear
    }

    if(!isNaN(newMonth)) {
        month.value = newMonth - 1
    }
})

interface HolidayMap {
  [year: string]: {
    [month: number]: number[]
  }
}

// Prazniki, organizirani po dnevu in mesecu
const holidays = ref<HolidayMap>({})

const holidayFileUrl = "/prazniki.txt"

const holidayError = ref("")
// Pridobivanje in parsiranje seznama praznikov
fetchHolidays()

async function fetchHolidays() {

    holidayError.value = ""

    const response = await fetch(holidayFileUrl)

    if(!response.ok) {
        console.error("Napaka pri pridobivanju praznikov")
        holidayError.value = "Prišlo je do napake pri nalaganju praznikov."
        return
    }

    const text = await response.text()

    const holidayDateList = text.split(',')

    const holidayMap: HolidayMap = {}

    // Mapiranje praznika glede na leto ali ponavljanje in mesec
    holidayDateList.forEach((holiday) => {
        const holidayDateParts = holiday.split("-")

        const holidayYear = holidayDateParts[0] === "P" ? "P" : parseInt(holidayDateParts[0])
        const holidayMonth = parseInt(holidayDateParts[1])
        const holidayDay = parseInt(holidayDateParts[2])

        // Preverjanje formata datuma
        if((holidayYear !== "P" && isNaN(holidayYear)) || isNaN(holidayMonth) || isNaN(holidayDay)) {
            return
        }

        if(!holidayMap[holidayYear])  {
            holidayMap[holidayYear] = {}
        }
        if(!holidayMap[holidayYear][holidayMonth]) {
            holidayMap[holidayYear][holidayMonth] = []
        }
        holidayMap[holidayYear][holidayMonth].push(holidayDay)
    })

    holidays.value = holidayMap
}

// Aktualni prazniki za izbrani mesec in leto
const actualHolidays = computed(() => {

    const actualYear = year.value
    // Dodajanje 1 zaradi indeksiranja mesecev, ki se začne z 0
    const actualMonth = month.value + 1

    let actualHolidayList: number[] = []

    if (holidays.value[actualYear] && holidays.value[actualYear][actualMonth]) {
        actualHolidayList = actualHolidayList.concat(holidays.value[actualYear][actualMonth])
    }
    if (holidays.value["P"] && holidays.value["P"][actualMonth]) {
        actualHolidayList = actualHolidayList.concat(holidays.value["P"][actualMonth])
    }

    return actualHolidayList
})

</script>

<style scoped>

.calendar-container {
    margin: auto;
    padding: 3rem 2rem;
    max-width: 1200px;
}

.calendar-head {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1.5rem 0rem;
}

input, select {
    border: none;
    background: none;
    font-size: 1.125rem;
    font-weight: bold;
    font-family: inherit;
}

#yearInput {
    max-width: 4.5rem;
}

.calendar-controls {
    display: flex;
    gap: 1rem;
}

#dateInput {
    padding: 0.75rem 1.25rem;
    border-radius: 0.5rem;
    background-color: #08080f;
    border: 1px solid #08080f;
    color: #f8f8ff;
    transition: color 0.125s ease, background-color 0.25s ease;
    cursor: text;
}

#dateInput:hover {
    color: #08080f;
    background-color: #f8f8ff;
}

.month-year-selector {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0.5rem 1rem 0.5rem 0.5rem;
    border-radius: 0.5rem;
    gap: 0.5rem;
    border: 1px solid #58585f;
    text-align: end;
}

.month-year-selector * {
    transition: color 0.125s ease, background-color 0.25s ease;
    padding: 0.25rem;
    border-radius: 0.25rem;
}

.month-year-selector *:hover {
    background-color: #08080f;
    color: #f8f8ff;
}

.error {
    color: #ff3c00
}

@media screen and (max-width: 900px) {
    .calendar-container {
        padding: 3rem 0.5rem;
    }
    .calendar-head {
        flex-direction: column;
        justify-content: space-between;
        align-items: start;
    }
    .calendar-controls {
        padding-top: 1rem;
        gap: 0.5rem;
    }

    input, select {
        font-size: 0.875rem;
    }

    #dateInput {
        padding: 0.5rem 0.75rem;
        background-color: #08080f;
        color: #f8f8ff;
        max-width: 8rem;
    }

    #yearInput {
        max-width: 3rem;
    }

    .month-year-selector {
        padding: 0.25rem 0.75rem 0.25rem 0.75rem;
        gap: 0.25rem;
    }
    
}



</style>