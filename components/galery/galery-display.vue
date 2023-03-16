<template>
  <div class="container-fluid mx-auto flex-grow-1 mt-2 py-5">
    <div class="row">
      <div class="col-md-40 col-sm-12 mx-auto">
        <div class="card">
          <div class="card-header">
            <h3 class="text-primary font-weight-bold font-size-lg">
              Previous searches
            </h3>
          </div>
          <div class="py-3">
            <div
              v-for="documentToDisplay in documentsToDisplay"
              :key="documentToDisplay._id"
            >
              <DocumentsToDisplay :document-to-display="documentToDisplay" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import API from '../../services/index.js'
import DocumentsToDisplay from './documents-display'

export default {
  components: { DocumentsToDisplay },
  data: function () {
    return {
      documentsToDisplay: [],
    }
  },
  mounted: async function () {
    const response = await API.get('/images/uploads')
    this.documentsToDisplay = response.data
    console.log(response.data)
  },
}
</script>

<style></style>
