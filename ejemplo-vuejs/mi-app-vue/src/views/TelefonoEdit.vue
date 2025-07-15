<template>
  <div>
    <h2>Editar Telefono</h2>
    <p v-if="loading">Cargando detalles...</p>
    <p v-if="error" class="error-message">{{ error }}</p>
    <div v-else-if="telefono">
      <form @submit.prevent="updateTelefono">
        <div class="form-group">
          <label for="telefono">Telefono:</label>
          <input
            type="text"
            id="telefono"
            v-model="telefono.telefono"
            required
          />
        </div>
        <div class="form-group">
          <label for="tipo">Tipo:</label>
          <input type="text" id="tipo" v-model="telefono.tipo" required />
        </div>
        <button type="submit">Actualizar</button>
      </form>
    </div>
    <p v-else>Telefono no encontrado.</p>

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
      telefono: null,
      loading: true,
      error: null,
    };
  },
  async created() {
    console.log("telefonoUrl recibida:", this.telefonoUrl);
    await this.fetchTelefonoDetail(this.telefonoUrl);
  },
  methods: {
    async fetchTelefonoDetail(url) {
      try {
        this.loading = true;
        this.error = null;
        const response = await api.get(url);
        this.telefono = response.data;
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
    async updateTelefono() {
      try {
        this.loading = true;
        this.error = null;
        const response = await api.put(this.telefonoUrl, this.telefono);
        this.telefono = response.data;
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
