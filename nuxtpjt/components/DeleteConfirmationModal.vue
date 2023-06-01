<template>
  <div v-if="show">
    <h3>삭제 확인</h3>
    <p>정말로 선택한 항목을 삭제하시겠습니까?</p>
    <button @click="confirm">확인</button>
    <button @click="cancel">취소</button>
  </div>
</template>

<script>
export default {
  props: {
    show: Boolean,
  },
  methods: {
    confirm() {
      this.$emit('confirm')
    },
    cancel() {
      this.$emit('cancel')
    },
  },
}
</script>
