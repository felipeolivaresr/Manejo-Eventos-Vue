<template>
  <div>
    <form @submit.prevent="agregarCita">
      <div>
        <label :style="{ color: form.paciente ? 'black' : 'red' }">Paciente</label>
        <input v-model="form.paciente" />
      </div>

      <div>
        <label :style="{ color: form.fecha ? 'black' : 'red' }">Fecha</label>
        <input v-model="form.fecha" type="date" />
      </div>

      <div>
        <label :style="{ color: form.hora ? 'black' : 'red' }">Hora</label>
        <input v-model="form.hora" type="time" />
      </div>

      <div>
        <label :style="{ color: form.gravedad ? 'black' : 'red' }">Gravedad</label>
        <select v-model="form.gravedad">
          <option disabled value="">Seleccione Gravedad</option>
          <option v-for="option in ['Baja', 'Media', 'Alta']" :key="option">{{ option }}</option>
        </select>
      </div>

      <div>
        <label :style="{ color: form.motivo ? 'black' : 'red' }">Motivo</label>
        <input v-model="form.motivo" />
      </div>

      <button :disabled="!formValido">Agregar Cita</button>
    </form>

    <p v-if="citas.length === 0">No hay consultas registradas</p>

    <div v-else class="cards-container">
      <!-- Usar el componente hijo para las tarjetas -->
      <CitaCard v-for="(cita, index) in citas" 
                :key="index" 
                :cita="cita" 
                @eliminar="eliminarCita(index)" />
    </div>
  </div>
</template>

<script>
import CitaCard from './components/Cita.vue';

export default {
  components: { CitaCard },
  data() {
    return {
      form: {
        paciente: '',
        fecha: '',
        hora: '',
        gravedad: '',
        motivo: ''
      },
      citas: []
    };
  },
  computed: {
    formValido() {
      return Object.values(this.form).every(value => value);
    }
  },
  methods: {
    agregarCita() {
      this.citas.push({ ...this.form });
      this.form = { paciente: '', fecha: '', hora: '', gravedad: '', motivo: '' };
    },
    eliminarCita(index) {
      this.citas.splice(index, 1);
    }
  }
};
</script>

<style>
.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.card {
  padding: 10px;
  margin: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 200px;
}
</style>