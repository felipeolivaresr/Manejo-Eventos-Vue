<template>
  <div>
    <div class="hero-section">
      <img src="./assets/img/consultorio.jpg" alt="Consultorio Médico" class="hero-image">
      <div class="hero-text">
        <h1>Consultorio Médico</h1>
      </div>
    </div>
    <div>
      <form @submit.prevent="agregarCita">
        <div>
          <label :style="{ color: form.paciente ? 'black' : 'red' }">Paciente</label>
          <input v-model="form.paciente" @blur="validateField('paciente')" />
          <span v-if="errors.paciente" class="error">{{ errors.paciente }}</span>
        </div>

        <div>
          <label :style="{ color: form.fecha ? 'black' : 'red' }">Fecha</label>
          <input v-model="form.fecha" type="date" @blur="validateField('fecha')" />
          <span v-if="errors.fecha" class="error">{{ errors.fecha }}</span>
        </div>

        <div>
          <label :style="{ color: form.hora ? 'black' : 'red' }">Hora</label>
          <input v-model="form.hora" type="time" @blur="validateField('hora')" />
          <span v-if="errors.hora" class="error">{{ errors.hora }}</span>
        </div>

        <div>
          <label :style="{ color: form.gravedad ? 'black' : 'red' }">Gravedad</label>
          <select v-model="form.gravedad" @blur="validateField('gravedad')">
            <option disabled value="">Seleccione Gravedad</option>
            <option v-for="option in ['Baja', 'Media', 'Alta']" :key="option">{{ option }}</option>
          </select>
          <span v-if="errors.gravedad" class="error">{{ errors.gravedad }}</span>
        </div>

        <div>
          <label :style="{ color: form.motivo ? 'black' : 'red' }">Motivo</label>
          <input v-model="form.motivo" @blur="validateField('motivo')" />
          <span v-if="errors.motivo" class="error">{{ errors.motivo }}</span>
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
      citas: [],
      errors: {}
    };
  },
  computed: {
    formValido() {
      return Object.values(this.form).every(value => value) && Object.keys(this.errors).length === 0;
    }
  },
  methods: {
    agregarCita() {
      this.citas.push({ ...this.form });
      this.form = { paciente: '', fecha: '', hora: '', gravedad: '', motivo: '' };
    },
    eliminarCita(index) {
      this.citas.splice(index, 1);
    },
    validateField(field) {
      if (!this.form[field]) {
        this.$set(this.errors, field, `El campo ${field} es obligatorio.`);
      } else {
        this.$delete(this.errors, field);
      }
    }
  }
};
</script>

<style>
.cards-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}

.card {
  padding: 20px;
  margin: 10px;
  border: 1px solid #ccc;
  border-radius: 10px;
  width: 250px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s, box-shadow 0.2s;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

form {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

form div {
  margin-bottom: 15px;
}

form label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

form input, form select {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  width: 100%;
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.2s;
}

button:disabled {
  background-color: #ccc;
}

button:hover:not(:disabled) {
  background-color: #0056b3;
}

.error {
  color: red;
  font-size: 0.8em;
}

/* Estilo para el título principal */
h1 {
  font-size: 2.5em;
  color: #007bff;
  text-align: center;
  margin-top: 20px;
  margin-bottom: 20px;
  font-family: 'Arial', sans-serif;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
}

.hero-section {
  position: relative;
  width: 100%;
  height: 200px; /* Reducir la altura mínima */
  overflow: hidden;
}

.hero-image {
  width: 100%;
  height: 300%;
  object-fit: cover;
  filter: brightness(70%);
}

.hero-text {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  text-align: center;
}

.hero-text h1 {
  font-size: 2.5em; /* Reducir el tamaño del texto */
  margin: 0;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

@media (max-width: 768px) {
  .hero-text h1 {
    font-size: 2em;
  }
}

@media (max-width: 480px) {
  .hero-text h1 {
    font-size: 1.5em;
  }
}
</style>