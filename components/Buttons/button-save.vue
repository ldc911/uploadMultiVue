<template>
  <b-col lg="2" class="d-flex justify-content-center"
    ><b-button class="validateButton" size="xs" @click="handleSave"
      >Sauvegarder</b-button
    ></b-col
  >
</template>

<script>
export default {
  methods: {
    handleSave: function () {
      this.$emit('save')
    },
  },
}
</script>
