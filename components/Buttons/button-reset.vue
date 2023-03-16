<template>
  <b-col lg="2" class="pb-2 m-2"
    ><b-button class="resetButton" size="xs" @click="handleReset"
      >Annuler</b-button
    ></b-col
  >
</template>

<script>
export default {
  methods: {
    handleReset() {
      this.$emit('reset')
    },
  },
}
</script>
