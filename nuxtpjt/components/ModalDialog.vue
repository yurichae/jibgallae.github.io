<template>
  <div class="modal">
    <div class="modal-contents">
      <div class="top">
        <h2>상세페이지</h2>
        <button class="btn" @click="closeModal">닫기</button>
      </div>
      <div class="center"></div>
      <div class="bottom"></div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'ModalDialog',
  methods: {
    closeModal() {
      this.$emit('closeModal')
    },
  },
}
</script>
