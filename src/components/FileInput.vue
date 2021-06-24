<template>
  <input class="file-input" accept="image/*" type="file" @change="changed" />
</template>
<script>
export default {
  name: "FileInput",
  methods: {
    changed(e) {
      const control = e.target;
      const [file] = control.files;
      this.$emit("added", file);
    },
  },
};
</script>
<style lang="scss" scoped>
.file-input {
  opacity: 0;
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  cursor: pointer;
  z-index: 1;
}
</style>
