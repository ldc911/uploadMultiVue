<!-- eslint-disable no-unused-vars -->
<template>
  <div class="single form-group">
    <label for="" class="font-size-sm font-weight-bold text-primary">
      {{ label }}
    </label>
    <input
      ref="uploadInput"
      type="file"
      class="form-control upload-input"
      @change="handleChange()"
    />
    <div class="uploader-mask d-flex justify-content-center align-items-center">
      <img src="@/assets/uploader.png" class="uploader-icon img-fluid" alt="" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { baseURL } from '~/constants'

// import API from '../../services/index.js'

export default {
  props: {
    label: {
      type: String,
      required: false,
      default: 'Upload single file',
    },
  },
  methods: {
    async handleChange() {
      const file = this.$refs.uploadInput.files[0]
      const formData = new FormData()
      formData.append('file', file)
      // eslint-disable-next-line no-unused-vars
      const res = await axios.post(`${baseURL}/images/single-upload`, formData)
      console.log('response from back', res)
    },
  },
}
</script>
