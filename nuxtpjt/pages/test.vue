<template>
    <div>
      <h1 class="logo"><img src="../assets/images/logo.png" alt="" /></h1>
      <div class="cont-wrap">
        <Tutorial :modalOpen="ModalOpen" @openModal="openModal" />
      </div>
      <ModalDialog v-if="ModalOpen == true" @closeModal="closeModal" />
    </div>
  </template>
  
  <script>
  import ModalDialog from '../components/ModalDialog.vue'
  export default {
    name: 'IndexPage',
    data() {
      return {
        ModalOpen: false,
      }
    },
    components: { ModalDialog },
    methods: {
      openModal() {
        this.ModalOpen = true
      },
      closeModal() {
        this.ModalOpen = false
      },
    },
  }
  </script>
  