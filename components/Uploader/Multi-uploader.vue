<template>
  <div class="multi form-group">
    <label :for="id" class="font-size-sm font-weight-bold text-primary">
      {{ label }}
    </label>
    <div class="d-flex">
      <div>
        <input
          :id="id"
          ref="uploadInput"
          multiple
          type="file"
          class="form-control upload-input"
          @change="handleChange()"
        />
        <div
          class="uploader-mask d-flex justify-content-center align-items-center"
        >
          <img
            src="@/assets/uploader.png"
            class="uploader-icon img-fluid"
            alt=""
          />
        </div>
      </div>
      <div v-if="isUploading" class="flex-grow-1 mx-2">
        <b-progress :value="uploadProgress" :max="100" show-progress animated />
      </div>
    </div>
    <div class="d-flex flex-wrap mt-3">
      <img
        v-for="uploadedImg in uploadedImgs"
        :key="uploadedImg"
        :src="uploadedImg"
        class="img-fluid img-thumbnail uploaded-img mr-2 mt-2"
        alt="Alternative text for uploaded image"
      />
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import API from '../../services/index.js'

export default {
  props: {
    label: {
      type: String,
      required: false,
      default: 'Upload single file',
    },
    id: {
      type: String,
      required: true,
    },
  },
  data: () => ({
    uploadedImgs: [],
    isUploading: false,
    uploadProgress: 0,
  }),
  methods: {
    async handleChange() {
      const { files } = this.$refs.uploadInput
      const formData = new FormData()
      _.forEach(files, (file) => {
        formData.append('files', file)
      })

      this.isUploading = true
      const { data } = await API.post(`/images/multi-upload`, formData, {
        onUploadProgress: ({ total, loaded }) => {
          this.uploadProgress = ((loaded / total) * 100).toFixed(2)
        },
      })
      this.uploadedImgs = [...this.uploadedImgs, ...data]
      this.isUploading = false
      this.uploadProgress = 0
    },
  },
}
</script>
