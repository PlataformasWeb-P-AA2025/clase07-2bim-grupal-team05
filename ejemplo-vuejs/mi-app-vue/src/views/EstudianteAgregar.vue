<template>
  <div class="estudiante-agregar-container">
    <h2>Agregar Estudiante</h2>

    <p v-if="loading">Guardando estudiante...</p>
    <p v-if="error" class="error-message">{{ error }}</p>

    <form @submit.prevent="agregarEstudiante" v-if="!loading">
      <div class="form-group">
        <label for="nombre">Nombre:</label>
        <input type="text" id="nombre" v-model="estudiante.nombre" required />
      </div>

      <div class="form-group">
        <label for="apellido">Apellido:</label>
        <input
          type="text"
          id="apellido"
          v-model="estudiante.apellido"
          required
        />
      </div>

      <div class="form-group">
        <label for="cedula">Cédula:</label>
        <input type="text" id="cedula" v-model="estudiante.cedula" required />
      </div>

      <div class="form-group">
        <label for="correo">Correo:</label>
        <input type="email" id="correo" v-model="estudiante.correo" required />
      </div>

      <button type="submit" class="submit-button">Agregar</button>
    </form>

    <router-link :to="{ name: 'EstudiantesList' }" class="back-button">
      Volver
    </router-link>
  </div>
</template>

<script>
import api from "@/api/axios";

export default {
  name: "EstudianteAgregar",
  data() {
    return {
      estudiante: {
        nombre: "",
        apellido: "",
        cedula: "",
        correo: "",
      },
      loading: false,
      error: null,
    };
  },
  methods: {
    async agregarEstudiante() {
      try {
        this.loading = true;
        this.error = null;
        await api.post("estudiantes/", this.estudiante);
        this.$router.push({ name: "EstudiantesList" }); // Redirige al listado
      } catch (err) {
        console.error("Error al agregar el estudiante:", err.response || err);
        this.error = "No se pudo agregar el estudiante. Verifique los datos.";
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

<style scoped>
.estudiante-agregar-container {
  max-width: 600px;
  margin: 50px auto;
  padding: 20px;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h2 {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.submit-button {
  width: 100%;
  padding: 10px;
  background-color: #28a745;
  border: none;
  color: white;
  font-weight: bold;
  border-radius: 5px;
  cursor: pointer;
}

.submit-button:hover {
  background-color: #218838;
}

.back-button {
  display: block;
  width: fit-content;
  margin: 20px auto 0;
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
  border-radius: 5px;
  text-decoration: none;
  text-align: center;
}

.back-button:hover {
  background-color: #0056b3;
}

.error-message {
  color: red;
  text-align: center;
  margin-bottom: 10px;
}
</style>
