<template>
  <div v-if="show">
    <h3>할 일 수정</h3>
    <input v-model="updatedTask" type="text" />
    <button @click="save">저장</button>
    <button @click="cancel">취소</button>
  </div>
</template>

<script>
export default {
  props: {
    task: String,
    index: Number,
    show: Boolean,
  },
  data() {
    return {
      updatedTask: '',
    }
  },
  mounted() {
    this.updatedTask = this.task
  },
  methods: {
    save() {
      this.$emit('save', this.updatedTask)
    },
    cancel() {
      this.$emit('cancel')
    },
  },
}
</script>
