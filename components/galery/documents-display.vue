<template>
  <div class="d-flex justify-content-center my-2">
    <div class="card w-75 shadow-sm">
      <div class="card-body d-flex justify-content-around align-items-center">
        <img
          :src="documentToDisplay.path"
          alt="myPic"
          class="img-fluid img-thumbnail uploaded-img mr-4 mt-2"
        />
        <div>AI found {{ responseDisplay(documentToDisplay.label) }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    documentToDisplay: {
      type: Object,
      default: () => {},
    },
  },
  methods: {
    responseDisplay(label) {
      if (label.length === 0) {
        return 'nothing on this image'
      }
      const response = label.reduce(
        (acc, { title, confidence, count }, index) => {
          const confidenceResponse = `with ${(
            confidence * 100
          ).toFixed()}% confidence`
          const entireResponse = `${count} ${title}(s) ${confidenceResponse}`

          if (index === 0) {
            return entireResponse
          } else if (index === label.length - 1) {
            return `${acc}, and ${entireResponse}`
          } else {
            return `${acc}, ${entireResponse}`
          }
        },
        ''
      )
      return response
    },
  },
}
</script>

<style></style>
