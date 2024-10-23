<template>
    <div :style="{ backgroundColor: colorSegunGravedad(cita.gravedad) }" class="card">
      <p>Paciente: {{ cita.paciente }}</p>
      <p>Fecha: {{ cita.fecha }}</p>
      <p>Hora: {{ cita.hora }}</p>
      <p>Gravedad: {{ cita.gravedad }}</p>
      <p>Motivo: {{ cita.motivo }}</p>
      <button @click="$emit('eliminar')">Eliminar</button>
    </div>
  </template>
  
  <script>
  export default {
    props: ['cita'],
    methods: {
      colorSegunGravedad(gravedad) {
        return {
          'Baja': 'green',
          'Media': 'yellow',
          'Alta': 'red'
        }[gravedad] || 'white';
      }
    }
  };
  </script>

<style>
.card {
  padding: 10px;
  margin: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 200px;
}
</style>