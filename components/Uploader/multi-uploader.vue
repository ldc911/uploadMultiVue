<template>
  <div class="multi form-group">
    <label :for="id" class="font-size-sm font-weight-bold text-primary">
      {{ label }}
    </label>
    <div>
      <input
        :id="id"
        ref="uploadInput"
        multiple
        type="file"
        class="form-control upload-input"
        @change="handleChange"
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
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  props: {
    label: {
      type: String,
      required: false,
      default: 'Upload multiple files',
    },
    id: {
      type: String,
      required: true,
    },
  },
  data: function () {
    return {
      picSelection: [],
    }
  },

  methods: {
    handleChange() {
      const { files } = this.$refs.uploadInput
      _.forEach(files, (file) => {
        file.fileId = this.generateId()
        const url = URL.createObjectURL(file)
        this.picSelection.push({ file, url })
      })
      console.log(this.picSelection)
      this.$emit('on-change', this.picSelection)
      this.picSelection = []
    },

    generateId() {
      return Math.random().toString()
    },
  },
}
</script>
