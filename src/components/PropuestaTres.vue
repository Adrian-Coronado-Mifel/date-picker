<template>
    <div class="form-group">
        <label>Propuesta 3:</label>
        <div class="input-birthday">
            <select v-model="selectedDay" @change="validateField">
                <option value="">Dia</option>
                <option v-for="number in 31" :key="number" :value="number">
                    {{ number }}
                </option>
            </select>
            <select v-model="selectedMonth" @change="validateField">
                <option value="">Mes</option>
                <option v-for="number in 12" :key="number" :value="number">
                    {{ number }}
                </option>
            </select>
            <select v-model="selectedYear" @change="validateField">
                <option value="">Año</option>
                <option v-for="year in yearOptions" :key="year" :value="year">
                    {{ year }}
                </option>
            </select>
        </div>
        <span class="error" v-if="error">{{ error }}</span>
    </div>
</template>

<script>
import { parseISO, isValid, } from 'date-fns';
export default {
    name: "PropuestaTres",
    data() {
        return {
            selectedDay: '',
            selectedMonth: '',
            selectedYear: '',
            error: "",
        };
    },
    computed: {
        yearOptions() {
            const currentYear = new Date().getFullYear();
            const startYear = currentYear - 100;
            const endYear = currentYear - 18; // Hasta el año actual menos 18
            const years = [];

            for (let year = startYear; year <= endYear; year++) {
                years.push(year);
            }

            return years;
        },
    },
    methods: {
        validateField() {
            if (this.selectedDay !== 'Dia' && this.selectedMonth !== 'Mes' && this.selectedYear !== 'Año') {
                // Asegúrate de que todos los valores sean tratados como cadenas
                const dayString = String(this.selectedDay).padStart(2, '0');
                const monthString = String(this.selectedMonth).padStart(2, '0');
                const yearString = String(this.selectedYear); // Asumiendo que el año ya tiene 4 dígitos

                // Construye la fecha ISO string
                const dateString = `${yearString}-${monthString}-${dayString}`;
                const date = parseISO(dateString);

                // Verifica si la fecha es válida
                if (isValid(date) && this.doesDateMatchInputs(date)) {
                    this.error = ''; // Fecha válida
                } else {
                    this.error = 'La fecha no es válida.';
                }
            }
        },
        doesDateMatchInputs(date) {
            // Verifica si el día, mes y año coinciden después de la construcción de la fecha
            const dayMatches = date.getDate() == parseInt(this.selectedDay);
            const monthMatches = date.getMonth() + 1 == parseInt(this.selectedMonth); // getMonth() es 0-index
            const yearMatches = date.getFullYear() == parseInt(this.selectedYear);

            return dayMatches && monthMatches && yearMatches;
        },
    },
};
</script>

<style scoped>
.input-birthday {
    display: flex;
    gap: 8px;
    margin-top: 20px;
}

input {
    width: 60px;
    border-radius: 5px;
    text-align: center;
    border: 1px solid #818992;
}

.error {
    color: red;
    margin-top: 5px;
    font-size: 12px;
    text-align: center;
}
</style>
