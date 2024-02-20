<template>
  <div class="form-group">
    <label>Fecha de Nacimiento:</label>
    <div class="input-birthday">
      <input type="text" placeholder="Día" v-model="day" @blur="() => validateField('day', 1, 31, 2)" />
      <input type="text" placeholder="Mes" v-model="month" @blur="() => validateField('month', 1, 12, 2)" />
      <input type="text" placeholder="Año" v-model="year"
        @blur="() => validateField('year', new Date().getFullYear() - 100, new Date().getFullYear() - 18, 4)" />
    </div>
    <span class="error" v-if="error">{{ error }}</span>
  </div>
</template>

<script>
import { parse, isValid, getDaysInMonth } from 'date-fns';

export default {
  name: "PropuestaUno",
  props: {
    date: Object,
  },
  data() {
    return {
      day: '',
      month: '',
      year: '',
      error: '',
    };
  },
  created() {
    if (this.date) {
      this.day = this.date.day;
      this.month = this.date.month;
      this.year = this.date.year;
    }
  },
  watch: {
    day(newValue) {
      const numericValue = newValue.replace(/[^\d]/g, '');
      if (numericValue !== newValue || numericValue.length > 2) {
        this.day = numericValue.slice(0, 2);
      }
    },
    month(newValue) {
      const numericValue = newValue.replace(/[^\d]/g, '');
      if (numericValue !== newValue || numericValue.length > 2) {
        this.month = numericValue.slice(0, 2);
      }
    },
    year(newValue) {
      const numericValue = newValue.replace(/[^\d]/g, '');
      if (numericValue !== newValue || numericValue.length > 4) {
        this.year = numericValue.slice(0, 4);
      }
    },
  },
  methods: {
    validateDateComponents() {
      const currentYear = new Date().getFullYear();
      const minYear = currentYear - 100;
      const maxYear = currentYear - 18;

      // Validación básica de componentes
      if (!this.day || !this.month || !this.year) {
        return 'Todos los campos deben ser llenados.';
      }
      
      // Validación de rango de año
      const numericYear = parseInt(this.year, 10);
      if (numericYear < minYear || numericYear > maxYear) {
        this.year = '';
        return 'El año está fuera del rango permitido.';
      }
      
      return '';
    },
    validateField() {
      // Limpia el error previo
      this.error = this.validateDateComponents();
      if (this.error) return;

      // Construcción y validación de la fecha casos especiales como anios bisciestos
      const dateString = `${this.year}-${this.month}-${this.day}`;
      const date = parse(dateString, 'yyyy-M-d', new Date());

      if (!isValid(date) || parseInt(this.day, 10) > getDaysInMonth(date)) {
        this.error = 'La fecha no es válida.';
        return;
      }

      // Fecha válida
      this.$emit('update:fecha', `${this.day.padStart(2, '0')}/${this.month.padStart(2, '0')}/${this.year}`);
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
