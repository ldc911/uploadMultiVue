<template>
  <div class="single form-group">
    <label for="" class="font-size-sm font-weight-bold text-primary">
      {{ label }}
    </label>
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      required: false,
      default: 'Upload single file',
    },
  },
}
</script>
