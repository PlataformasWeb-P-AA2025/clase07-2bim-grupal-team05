<template>
  <div>
    <h2>Eliminar Telefono</h2>
    <p>¿Estás seguro de eliminar este telefono?</p>
    <button @click="deleteTelefono">Eliminar</button>
    <button @click="cancelar">Cancelar</button>
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
        console.log("URL a eliminar:", this.telefonoUrl);
        await api.delete(this.telefonoUrl);
        this.$router.push({
          name: "EstudianteDetail",
          params: { estudianteUrl: this.estudianteUrl },
        });
      } catch (err) {
        console.error("Error al eliminar el teléfono:", err.response || err);
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
