<template>
  <div>
    <input v-model="newTask" type="text" placeholder="할 일 입력" />
    <button @click="addTask">추가</button>

    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        {{ task }}
        <button @click="editTask(index)">수정</button>
        <button @click="confirmDelete(index)">삭제</button>
      </li>
    </ul>

    <EditTaskModal
      :task="selectedTask"
      :index="selectedIndex"
      :show="showEditModal"
      @save="saveTask"
      @cancel="cancelEdit"
    />

    <DeleteConfirmationModal
      :show="showDeleteModal"
      @confirm="deleteTask"
      @cancel="cancelDelete"
    />
  </div>
</template>

<script>
import EditTaskModal from '../components/EditTaskModal.vue'
import DeleteConfirmationModal from '../components/DeleteConfirmationModal.vue'

export default {
  data() {
    return {
      newTask: '',
      tasks: [],
      selectedTask: '',
      selectedIndex: -1,
      showEditModal: false,
      showDeleteModal: false,
    }
  },
  methods: {
    addTask() {
      if (this.newTask) {
        this.tasks.push(this.newTask)
        this.newTask = ''
      }
    },
    editTask(index) {
      this.selectedTask = this.tasks[index]
      this.selectedIndex = index
      this.showEditModal = true
    },
    saveTask(updatedTask) {
      if (updatedTask) {
        this.tasks.splice(this.selectedIndex, 1, updatedTask)
      }
      this.showEditModal = false
    },
    cancelEdit() {
      this.showEditModal = false
    },
    confirmDelete(index) {
      this.selectedTask = this.tasks[index]
      this.selectedIndex = index
      this.showDeleteModal = true
    },
    deleteTask() {
      this.tasks.splice(this.selectedIndex, 1)
      this.showDeleteModal = false
    },
    cancelDelete() {
      this.showDeleteModal = false
    },
  },
  components: {
    EditTaskModal,
    DeleteConfirmationModal,
  },
}
</script>
