<template>
  <div class="container flex-grow-1 mt-5 py-5">
    <div class="row">
      <div class="col-md-6 col-sm-12 mx-auto">
        <div class="card">
          <div class="card-header">
            <h3 class="text-primary font-weight-bold font-size-lg">
              Upload your files
            </h3>
          </div>
          <div class="card-body">
            <div class="d-flex flex-row align-items-center">
              <form>
                <MultiUploader
                  id="multi-files-upload"
                  label="Upload multiple files here"
                  @on-change="onChange"
                />
              </form>
              <div v-if="isUploading" class="flex-grow-1 mx-2">
                <b-progress
                  :value="uploadProgress"
                  :max="100"
                  show-progress
                  animated
                />
              </div>
            </div>
            <SelectedImgsDisplay :selected-imgs="selectedImgs" />
            <div v-if="selectedImgs.length > 0" class="d-flex flex-row">
              <ButtonSubmit @submit="handlePost" /><ButtonReset
                @reset="handleReset"
              />
            </div>
          </div>
        </div>
        <div v-if="uploadedImgs.length > 0" class="card mt-2">
          <div class="card-body">
            <div class="d-flex flex-column align-items-center">
              <ButtonSave @save="saveResults" />
              <div v-if="aiResultEmpty" class="mt-1">
                <b-alert show variant="warning"
                  >The result has already been saved !</b-alert
                >
              </div>
              <div v-if="saveSuccess" class="mt-5 position-absolute">
                <b-alert show variant="success"
                  >The result has been saved !</b-alert
                >
              </div>
              <div v-if="saveError" class="mt-1">
                <b-alert show variant="danger"
                  >Something went wrong, result not saved !</b-alert
                >
              </div>
            </div>

            <ResultDisplay
              :uploaded-imgs="uploadedImgs"
              :ai-result="aiResult"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import API from '../../services/index.js'
import MultiUploader from '../Uploader/multi-uploader'
import ResultDisplay from '../Uploader/result-display'
import ButtonReset from '../Buttons/button-reset'
import ButtonSubmit from '../Buttons/button-submit'
import ButtonSave from '../Buttons/button-save.vue'
import SelectedImgsDisplay from '../Uploader/selectedImgs-display'

export default {
  components: {
    ResultDisplay,
    MultiUploader,
    ButtonReset,
    ButtonSubmit,
    ButtonSave,
    SelectedImgsDisplay,
  },
  data: function () {
    return {
      wsConnexion: null,
      wsRoom: null,
      aiResult: [],
      selectedImgs: [],
      isUploading: false,
      uploadProgress: 0,
      uploadedImgs: [],
      fileId: null,
      dataToSave: {},
      aiResultEmpty: false,
      saveSuccess: false,
      saveError: false,
      aiResultSaved: false,
    }
  },
  mounted: function () {
    console.log('Starting wsConnexion to WebSocket Server')
    this.wsConnexion = new WebSocket('ws://localhost:8080')

    this.wsConnexion.onerror = (e) => {
      console.error(e)
    }

    this.wsConnexion.onopen = () => {
      const room = this.genereateWsRoom(5)
      this.wsRoom = room
      const obj = { type: 'create', params: { room } }
      this.wsConnexion.send(JSON.stringify(obj))
      console.log('Connexion to webSocket done on room %s', this.wsRoom)
    }

    this.wsConnexion.onmessage = (data) => {
      const response = JSON.parse(data.data.toString())
      this.aiResult.push({
        fileId: response.fileId,
        result: response.result,
        path: response.path,
      })
      this.selectedImgs = []
      this.isUploading = false
      this.uploadProgress = 0
      this.fileId = ''
      this.aiResultEmpty = false
      this.aiResultSaved = false
    }
  },
  methods: {
    genereateWsRoom(length) {
      let result = ''
      const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789'
      for (let i = 0; i < length; i++) {
        result += characters.charAt(
          Math.floor(Math.random() * characters.length)
        )
      }
      return result
    },

    onChange(picSelection) {
      this.selectedImgs = picSelection
      this.aiResult = []
      this.uploadedImgs = []
    },

    handleReset() {
      this.selectedImgs = []
      this.fileId = ''
    },

    showSuccess() {
      setTimeout(() => (this.saveSuccess = false), 2000)
    },

    async saveResults() {
      if (this.aiResult.length > 0 && !this.aiResultSaved) {
        this.dataToSave = { uploads: this.aiResult }
        const response = await API.post('images/uploads', this.dataToSave)
        response.status === 200
          ? (this.saveSuccess = true)
          : (this.saveError = true)
        this.showSuccess()
        this.aiResultSaved = true
      } else {
        this.aiResultEmpty = true
      }
    },

    async handlePost() {
      this.aiResult = []
      const formData = new FormData()
      _.forEach(this.selectedImgs, (selectedImg) => {
        formData.append('files', selectedImg.file, selectedImg.file.fileId)
      })
      formData.set('wsRoom', this.wsRoom)

      this.isUploading = true
      await API.post(`/images/multi-upload`, formData, {
        onUploadProgress: ({ total, loaded }) => {
          this.uploadProgress = ((loaded / total) * 100).toFixed(2)
        },
      })
      this.uploadedImgs = this.selectedImgs
    },
  },
}
</script>

<style></style>
