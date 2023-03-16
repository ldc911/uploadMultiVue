<template>
  <div class="d-flex flex-wrap mt-1 mb-4">
    <img
      v-for="selectedImg in selectedImgs"
      :key="selectedImg.url"
      :src="selectedImg.url"
      class="img-fluid img-thumbnail uploaded-img mr-2 mt-2"
      alt="Alternative text for uploaded image"
    />
  </div>
</template>

<script>
export default {
  props: {
    selectedImgs: {
      type: Array,
      required: false,
      default: () => [],
    },
  },
}
</script>

<style></style>
