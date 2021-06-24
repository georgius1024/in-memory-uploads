<template>
  <img :src="src" class="preview">
</template>
<script>
export default {
  name: 'Preview',
  props: {
    src: {
      type: String,
      required: true
    }
  }
}
</script>
<style lang="scss" scoped>
  .preview {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    object-fit: cover;
  }

</style>