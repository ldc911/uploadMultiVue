<template>
  <div class="myApp d-flex flex-column">
    <Navbar />
    <ContentDisplay />
    <FooterItemVue />
  </div>
</template>

<script>
import FooterItemVue from '~/components/Layouts/footer-item'
import Navbar from '~/components/Layouts/nav-bar'
import ContentDisplay from '~/components/Uploader/content-display'

export default {
  name: 'IndexPage',
  components: {
    Navbar,
    FooterItemVue,
    ContentDisplay,
  },
}
</script>
