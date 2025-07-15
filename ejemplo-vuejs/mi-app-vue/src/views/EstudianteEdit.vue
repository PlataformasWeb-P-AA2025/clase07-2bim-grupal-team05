<template>
  <div>
    <h2>Editar Estudiante</h2>
    <p v-if="loading">Cargando detalles...</p>
    <p v-if="error" class="error-message">{{ error }}</p>
    <div v-else-if="estudiante">
      <form @submit.prevent="updateEstudiante">
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
          <input
            type="email"
            id="correo"
            v-model="estudiante.correo"
            required
          />
        </div>
        <button type="submit">Actualizar</button>
      </form>
    </div>
    <p v-else>Estudiante no encontrado.</p>

    <router-link
      :to="{
        name: 'EstudianteDetail',
        params: { estudianteUrl: estudianteUrl },
      }"
      class="back-button"
      >Volver</router-link
    >
  </div>
</template>

<script>
import api from "@/api/axios";

export default {
  name: "EstudianteEdit",
  props: ["estudianteUrl"],
  data() {
    return {
      estudiante: null,
      loading: true,
      error: null,
    };
  },
  async created() {
    await this.fetchEstudianteDetail(this.estudianteUrl);
  },
  methods: {
    async fetchEstudianteDetail(url) {
      try {
        this.loading = true;
        this.error = null;
        const response = await api.get(url);
        this.estudiante = response.data;
      } catch (err) {
        console.error(
          "Error al cargar detalle del estudiante:",
          err.response || err
        );
        this.error = "No se pudo cargar el detalle del estudiante.";
      } finally {
        this.loading = false;
      }
    },
    async updateEstudiante() {
      try {
        this.loading = true;
        this.error = null;
        const response = await api.put(this.estudianteUrl, this.estudiante);
        this.estudiante = response.data;
      } catch (err) {
        console.error(
          "Error al actualizar el estudiante:",
          err.response || err
        );
        this.error = "No se pudo actualizar el estudiante.";
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>
