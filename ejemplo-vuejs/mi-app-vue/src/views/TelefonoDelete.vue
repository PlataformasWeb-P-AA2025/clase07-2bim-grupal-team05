<template>
  <div class="telefono-delete-container">
    <h2>Eliminar Teléfono</h2>
    <p>¿Estás seguro de que deseas eliminar este número telefónico?</p>
    <div class="button-group">
      <button @click="deleteTelefono" class="delete-button">Eliminar</button>
      <button @click="cancelar" class="cancel-button">Cancelar</button>
    </div>
  </div>
</template>

<script>
import api from "@/api/axios";

export default {
  name: "TelefonoDelete",
  props: ["estudianteUrl", "telefonoUrl"],
  methods: {
    async deleteTelefono() {
      try {
        const decodedTelefonoUrl = decodeURIComponent(this.telefonoUrl);
        await api.delete(decodedTelefonoUrl);
        this.$router.push({
          name: "EstudianteDetail",
          params: { estudianteUrl: this.estudianteUrl },
        });
      } catch (err) {
        console.error("Error al eliminar el teléfono:", err.response || err);
        alert("No se pudo eliminar el teléfono. Intenta nuevamente.");
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
.telefono-delete-container {
  max-width: 500px;
  margin: 50px auto;
  padding: 25px;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 8px;
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

.button-group {
  display: flex;
  justify-content: center;
  gap: 20px;
}

button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  font-weight: bold;
  cursor: pointer;
  color: #fff;
}

.delete-button {
  background-color: #dc3545;
}

.delete-button:hover {
  background-color: #c82333;
}

.cancel-button {
  background-color: #6c757d;
}

.cancel-button:hover {
  background-color: #5a6268;
}
</style>
