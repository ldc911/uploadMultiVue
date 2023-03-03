<template>
  <div class="myApp d-flex flex-column">
    <Navbar />
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
              <form><SingleUploader /></form>
            </div>
          </div>
        </div>
      </div>
    </div>
    <FooterItemVue />
  </div>
</template>

<script>
import FooterItemVue from '~/components/Layouts/Footer-item.vue'
import Navbar from '~/components/Layouts/Nav-bar.vue'
import SingleUploader from '~/components/Uploader/Single-uploader.vue'

export default {
  name: 'IndexPage',
  components: { Navbar, FooterItemVue, SingleUploader },
}
</script>
