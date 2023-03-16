<template>
  <b-col lg="2" class="pb-2 m-2"
    ><b-button class="validateButton" size="xs" @click="handleSubmit"
      >Valider</b-button
    ></b-col
  >
</template>

<script>
export default {
  methods: {
    handleSubmit: function () {
      this.$emit('submit')
    },
  },
}
</script>
