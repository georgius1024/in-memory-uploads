<template>
  <div class="dropzone" v-if="!src">
    <p>Drop files there</p>
    <FileInput @added="fileAdded" />
  </div>
  <div class="preview" v-else>
    <ImagePreview :src="src" v-if="src" />
    <FileInput @added="fileAdded" />
  </div>
  <div class="navigation" v-show="files.length">
    <h4>Browse</h4>
    <div class="row">
      <button :disabled="index === 0" @click="index -= 1">&lt;</button>
      {{ index + 1 }} / {{ files.length }}
      <button :disabled="index === files.length - 1" @click="index += 1">
        &gt;
      </button>
    </div>
  </div>
  <div class="upload" v-show="src">
    <h4>Upload</h4>
    <input v-model="upload" placeholder="upload url" />
    <input v-model="method" placeholder="upload method" />
    <input v-model="field" placeholder="upload field" />
    <button v-show="src" @click="uploadSelected">Upload</button>
  </div>
</template>

<script>
import FileInput from "./components/FileInput.vue";
import ImagePreview from "./components/ImagePreview.vue";
import axios from "axios";
export default {
  name: "App",
  components: {
    FileInput,
    ImagePreview,
  },
  data() {
    return {
      files: [],
      index: -1,
      upload: "localhost",
      method: "patch",
      field: "image",
    };
  },
  computed: {
    src() {
      const file = this.files[this.index];
      if (file) {
        return URL.createObjectURL(file);
      }
    },
  },
  methods: {
    fileAdded(file) {
      this.files.push(file);
      this.index = this.files.length - 1;
      console.log(file, this.files.length);
    },
    uploadSelected() {
      const file = this.files[this.index];
      const formData = new FormData();
      formData.append(this.field, file);
      axios[this.method](this.upload, formData, {
        headers: {
          "Content-Type": "multipart/form-data",
        },
      });
    },
  },
};
</script>
<style lang="scss">
@import "https://fonts.googleapis.com/css?family=Roboto:300,400,600,700";
body,
td,
th {
  font-size: 16px;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Roboto, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  align-items: center;
  width: 100%;
  display: flex;
  flex-direction: column;
}
.dropzone {
  width: 200px;
  height: 100px;
  display: flex;
  position: relative;
  align-items: center;
  justify-content: center;
  border: 1px solid #333;
}
.preview {
  position: relative;
  width: 200px;
  height: 200px;
  display: flex;
}
.navigation {
  margin-top: 32px;
  .row {
    display: flex;
    justify-content: space-around;
    align-items: center;
    button {
      background-color: none;
      border: 1px solid #333;
      padding: 4px;
      margin: 4px;
    }
  }
}
.upload {
  margin-top: 32px;
  display: flex;
  flex-direction: column;
  align-content: space-around;
  button,
  input {
    background-color: none;
    border: 1px solid #333;
    padding: 4px;
    margin: 4px 0;
  }
}
</style>
