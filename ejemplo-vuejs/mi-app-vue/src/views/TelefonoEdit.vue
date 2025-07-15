<template>
  <div>
    <h2>Editar Telefono</h2>
    <p v-if="loading">Cargando detalles...</p>
    <p v-if="error" class="error-message">{{ error }}</p>
    <div v-else-if="estudiante">
      <form @submit.prevent="updateEstudiante">
        <div class="form-group">
          <label for="telefono">Telefono:</label>
          <input
            type="text"
            id="telefono"
            v-model="estudiante.telefono"
            required
          />
        </div>
        <div class="form-group">
          <label for="tipo">Tipo:</label>
          <input type="text" id="tipo" v-model="estudiante.tipo" required />
        </div>
        <button type="submit">Actualizar</button>
      </form>
    </div>
    <p v-else>Estudiante no encontrado.</p>

    <router-link
      :to="{
        name: 'EstudianteDetail',
        params: { estudianteUrl: estudianteUrl, telefonoUrl: telefonoUrl },
      }"
      class="back-button"
      >Volver</router-link
    >
  </div>
</template>

<script>
import api from "@/api/axios";

export default {
  name: "TelefonoEdit",
  props: ["estudianteUrl", "telefonoUrl"],
  data() {
    return {
      estudiante: null,
      loading: true,
      error: null,
    };
  },
  async created() {
    await this.fetchEstudianteDetail(this.telefonoUrl);
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
          "Error al cargar detalle del telefono:",
          err.response || err
        );
        this.error = "No se pudo cargar el detalle del telefono.";
      } finally {
        this.loading = false;
      }
    },
    async updateEstudiante() {
      try {
        this.loading = true;
        this.error = null;
        const response = await api.put(this.telefonoUrl, this.estudiante);
        this.estudiante = response.data;
      } catch (err) {
        console.error("Error al actualizar el telefono:", err.response || err);
        this.error = "No se pudo actualizar el telefono.";
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>
