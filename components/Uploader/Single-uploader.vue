<template>
  <div class="single form-group border-bottom">
    <label :for="id" class="font-size-sm font-weight-bold text-primary">
      {{ label }}
    </label>
    <div class="d-flex align-items-center">
      <div>
        <input
          :id="id"
          ref="uploadInput"
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
      <img
        v-if="uploadedImg.file"
        :src="uploadedImg.url"
        class="img-fluid img-thumbnail ml-3 uploaded-img"
        alt="Alternative text for uploaded image"
      />
      <div v-if="isUploading" class="flex-grow-1 mx-2">
        <b-progress :value="uploadProgress" :max="100" show-progress animated />
      </div>
    </div>
    <div class="d-flex flex-row">
      <ButtonSubmit @click="handlePost()" /><ButtonResetVue
        @reset="handleReset()"
      />
    </div>
  </div>
</template>

<script>
import API from '../../services/index.js'
import ButtonResetVue from '../Buttons/Button-reset.vue'
import ButtonSubmit from '../Buttons/Button-submit.vue'

export default {
  components: { ButtonSubmit, ButtonResetVue },
  props: {
    label: {
      type: String,
      required: false,
      default: 'Upload single file',
    },
    id: { type: String, required: true },
  },
  data: () => ({
    uploadedImg: {},
    isUploading: false,
    uploadProgress: 0,
  }),

  methods: {
    handleChange() {
      const file = this.$refs.uploadInput.files[0]
      const url = URL.createObjectURL(file)
      this.uploadedImg = { file, url }
    },
    async handlePost(e) {
      const { file } = this.uploadedImg
      const formData = new FormData()
      formData.append('file', file)
      this.isUploading = true
      const response = await API.post(`/images/single-upload`, formData, {
        onUploadProgress: ({ total, loaded }) => {
          this.uploadProgress = ((loaded / total) * 100).toFixed(2)
        },
      })
      console.log('voila la reponse', response)
      this.isUploading = false
      this.uploadProgress = 0
    },
    handleReset(e) {
      this.uploadedImg = {}
      console.log(this.uploadedImg)
    },
  },
}
</script>
