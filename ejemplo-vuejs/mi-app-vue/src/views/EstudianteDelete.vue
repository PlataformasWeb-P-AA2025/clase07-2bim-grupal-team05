<template>
  <div>
    <h2>Eliminar Estudiante</h2>
    <p>¿Estás seguro de eliminar este estudiante?</p>
    <button @click="deleteEstudiante">Eliminar</button>
    <button @click="cancelar">Cancelar</button>
  </div>
</template>

<script>
import api from "@/api/axios";

export default {
  name: "EstudianteDelete",
  props: ["estudianteUrl"],
  methods: {
    async deleteEstudiante() {
      try {
        await api.delete(this.estudianteUrl);
        this.$router.push({ name: "EstudiantesList" });
      } catch (err) {
        console.error("Error al eliminar el estudiante:", err.response || err);
      }
    },
    cancelar() {
      this.$router.push({
        name: "EstudianteDetail",
        params: { estudianteUrl: this.estudianteUrl },
      });
    },
  },
};
</script>
