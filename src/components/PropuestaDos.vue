<template>
    <div class="proposal-2">
        <label for="">Propuesta 2:</label>
        <input type="text" v-model="fecha" @input="formatFecha" @blur="validateDate" placeholder="dd/mm/aaaa" name="fecha">
        <span v-show="fechaError" class="error">{{ fechaErrorMsg }}</span>
    </div>
</template>

<script>
import { parse, isValid } from 'date-fns';

export default {
    name: 'PropuestaDos',
    data() {
        return {
            fecha: '',
            fechaError: false,
            fechaErrorMsg: '',
        };
    },
    methods: {
        formatFecha() {
            let valor = this.fecha.replace(/[^\d]/g, '');
            valor = valor.substring(0, 8);

            if (valor.length > 4) {
                valor = valor.substring(0, 2) + '/' + valor.substring(2, 4) + '/' + valor.substring(4, 8);
            } else if (valor.length > 2) {
                valor = valor.substring(0, 2) + '/' + valor.substring(2);
            }

            this.fecha = valor;
        },
        validateDate() {
            const regex = /^\d{2}\/\d{2}\/\d{4}$/;
            if (!regex.test(this.fecha)) {
                this.setFechaError('Formato de fecha incorrecto. Utilice dd/mm/aaaa');
                return;
            }

            const [day, month, year] = this.fecha.split('/').map(num => parseInt(num));
            const date = parse(this.fecha, 'dd/MM/yyyy', new Date(year, month - 1, day));
            const currentYear = new Date().getFullYear();
            const minYear = currentYear - 100;
            const maxYear = currentYear - 18;

            if (!isValid(date) || year < minYear || year > maxYear) {
                this.setFechaError('Ingrese una fecha valida');
                return;
            }

            // Si la fecha es válida, limpia los errores y emite la fecha
            this.fechaError = false;
            this.fechaErrorMsg = '';
            // Emitir evento con la fecha válida
            //console.log('Emitiendo fecha:', this.fecha);
            this.$emit('update:fecha', this.fecha);
        },
        setFechaError(msg) {
            this.fechaError = true;
            this.fechaErrorMsg = msg;
        },
    },
};
</script><script>
import { parse, isValid } from 'date-fns';

export default {
  name: 'PropuestaDos',
  data() {
    return {
      fecha: '',
      fechaError: false,
      fechaErrorMsg: '',
    };
  },
  methods: {
    formatFecha() {
      let valor = this.fecha.replace(/[^\d]/g, '');

      // Limita la longitud total a 8 dígitos numéricos para dd/mm/aaaa
      valor = valor.substring(0, 8);

      if (valor.length > 4) {
        // Asegura que solo se tomen los primeros 4 dígitos para el año
        valor = valor.substring(0, 2) + '/' + valor.substring(2, 4) + '/' + valor.substring(4, 8);
      } else if (valor.length > 2) {
        valor = valor.substring(0, 2) + '/' + valor.substring(2);
      }

      this.fecha = valor;
    },
    validateDate() {
      const regex = /^\d{2}\/\d{2}\/\d{4}$/;
      if (!regex.test(this.fecha)) {
        this.setFechaError('Formato de fecha incorrecto. Utilice dd/mm/aaaa');
        return;
      }

      const [day, month, year] = this.fecha.split('/').map(num => parseInt(num));
      const date = parse(this.fecha, 'dd/MM/yyyy', new Date(year, month - 1, day));
      const currentYear = new Date().getFullYear();
      const minYear = 1924;
      const maxYear = currentYear - 18;

      if (!isValid(date) || year < minYear || year > maxYear) {
        this.setFechaError('Ingrese una fecha valida');
        return;
      }

      this.fechaError = false;
      this.fechaErrorMsg = '';
        this.$emit('update:fecha', this.fecha);
    },
    setFechaError(msg) {
      this.fechaError = true;
      this.fechaErrorMsg = msg;
    },
  },
};
</script>

<style scoped>
.proposal-2 {
    
    display: grid;
}

input {
    margin-top: 12px;
    margin-left: 20px;
    border-radius: 5px;
    padding: 5px 10px;
    width: 150px;
}

span {
    font-size: 12px;
    text-align: left;
}

.error {
    color: red;
}
</style>
