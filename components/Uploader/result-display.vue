<template>
  <div>
    <div
      v-for="uploadedImg in uploadedImgs"
      :key="uploadedImg.url"
      class="d-flex align-items-center"
    >
      <img
        :src="uploadedImg.url"
        alt=""
        class="img-fluid img-thumbnail uploaded-img mr-2 mt-2"
      /><span v-if="aiResult.length === uploadedImgs.length">
        AI found
        {{ generateResponse(uploadedImg) }}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    uploadedImgs: {
      type: Array,
      required: false,
      default: () => [],
    },
    aiResult: {
      type: Array,
      required: false,
      default: () => [],
    },
  },
  methods: {
    generateResponse(data) {
      const response = this.aiResult.filter((element) => {
        return element.fileId === data.file.fileId
      })
      const obj = response[0].result
      const resultArray = []
      for (const key in obj) {
        const count = obj[key].count
        const avgConfidence = obj[key].averageConfidence.toFixed(2)

        resultArray.push({ key, count, avgConfidence })
      }
      if (resultArray.length === 0) {
        return 'nothing on this image'
      }
      const resultAI = resultArray.reduce(
        (acc, { key, count, avgConfidence }, index) => {
          const confidenceResponse = `with a ${(
            avgConfidence * 100
          ).toFixed()}% confidence`
          const entireResponse = `${count} ${key}(s) ${confidenceResponse}`

          if (index === 0) {
            return entireResponse
          } else if (index === resultArray.length - 1) {
            return `${acc}, and ${entireResponse}.`
          } else {
            return `${acc}, ${entireResponse}`
          }
        },
        ''
      )
      return resultAI
    },
  },
}
</script>

<style></style>
