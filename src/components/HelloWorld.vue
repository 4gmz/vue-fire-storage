<template>
  <div class="form-group col-sm-12">
    <label>adjunte un archivo</label>
    <input
      type="file"
      ref="adj"
      accept="*"
      :disabled="loading"
      class="form-control"
      id="archivoCedulaConyuge"
      data-validate="Anexe"
    >
    <br>
    <br>
    <br>
    <button :disabled="loading" @click="agregar">Subir</button>
    <p v-if="downloadUrl">
      Archivo disponible en:
      <a :href="downloadUrl">Link</a>
    </p>
  </div>
</template>
<script>
import firebase from "firebase";
import "firebase/storage";

export default {
  data() {
    return {
      loading: false,
      downloadUrl: ""
    };
  },
  methods: {
    async agregar() {
      try {
        const { files } = this.$refs.adj;
        this.loading = true;
        const file = files[0];
        if (file) {
          const response = await firebase
            .storage()
            .ref(`${file.name}`)
            .put(file);

          const url = await response.ref.getDownloadURL();

          console.log("archivo disponible en ", url);
          this.downloadUrl = url;
        } else {
          console.log("falta el archivo");
        }
      } catch (error) {
        console.error(error);
      }
      this.loading = false;
    }
  }
};
</script>