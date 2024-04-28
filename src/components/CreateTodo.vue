<script setup lang="ts">
import useTodos from '@/store/useTodos'
import type { Todo, TodoStatus } from '@/types'
import { reactive, ref } from 'vue'

interface Props {
  status: TodoStatus
}

const props = defineProps<Props>()
const shouldDisplayForm = ref(false)

const { addNewTodo } = useTodos()

const newTodo = reactive<Omit<Todo, 'id'>>({
  title: '',
  description: '',
  status: props.status
})

const resetForm = () => {
  shouldDisplayForm.value = false
  newTodo.title = ''
  newTodo.description = ''
}

const handleOnSubmit = () => {
  if (!newTodo.title) {
    newTodo.title = 'Default title'
  }
  if (!newTodo.description) {
    newTodo.description = 'Default description'
  }
  addNewTodo({
    id: Math.random() * 100000000000,
    ...newTodo
  })

  resetForm()
}
</script>

<template>
  <div>
    <h3 v-if="!shouldDisplayForm" @click="shouldDisplayForm = !shouldDisplayForm">Add New</h3>
    <template v-else>
      <form @submit.prevent="handleOnSubmit">
        <div>
          <input type="text" placeholder="Title" class="input" v-model="newTodo.title" />
        </div>
        <div>
          <textarea
            type="text"
            placeholder="Description"
            class="textarea"
            v-model="newTodo.description"
          />
        </div>
        <button type="submit">Submit</button>
        <button type="button" @click="resetForm">Cancel</button>
      </form>
    </template>
  </div>
</template>

<style scoped>
.input {
  width: 100%;
  margin-bottom: 3px;
}

.textarea {
  width: 100%;
}
</style>
