<template>
  <div class="myApp d-flex flex-column">
    <Navbar />
    <GaleryDisplay />
    <FooterItemVue />
  </div>
</template>

<script>
import GaleryDisplay from '../components/galery/galery-display'
import FooterItemVue from '~/components/Layouts/footer-item'
import Navbar from '~/components/Layouts/nav-bar'

export default {
  name: 'GaleryPage',
  components: {
    Navbar,
    FooterItemVue,
    GaleryDisplay,
  },
}
</script>

<style></style>
