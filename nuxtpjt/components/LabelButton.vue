<template>
  <div>
    <button class="custom-button" @click="btnClick">
      {{ label }}
    </button>
    <p v-if="showMessage">{{ message }}</p>
    <!-- v-if 조건부 렌더링 -->
  </div>
</template>
<script>
export default {
  props: {
    label: { type: String, required: true },
  },
  data() {
    return {
      showMessage: false,
      message: 'hi',
    }
  },
  methods: {
    btnClick() {
      this.$emit('button-clicked')
      this.showMessage = true
    },
  },
}
</script>
