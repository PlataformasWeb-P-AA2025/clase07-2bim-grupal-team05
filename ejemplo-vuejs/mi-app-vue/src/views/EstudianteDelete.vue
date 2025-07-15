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
<style scoped>
div {
  max-width: 500px;
  margin: 50px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #fff;
  text-align: center;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h2 {
  color: #dc3545;
  margin-bottom: 20px;
}

p {
  font-size: 16px;
  margin-bottom: 30px;
}

button {
  padding: 10px 20px;
  margin: 0 10px;
  border: none;
  border-radius: 5px;
  font-weight: bold;
  cursor: pointer;
  color: #fff;
}

button:first-of-type {
  background-color: #dc3545;
  /* rojo para eliminar */
}

button:first-of-type:hover {
  background-color: #c82333;
}

button:last-of-type {
  background-color: #6c757d;
  /* gris para cancelar */
}

button:last-of-type:hover {
  background-color: #5a6268;
}
</style>
