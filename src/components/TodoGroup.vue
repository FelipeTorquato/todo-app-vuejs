<script setup lang="ts">
import { TodoStatus } from '@/types'
import useTodos from '@/store/useTodos'
import Draggable from 'vuedraggable'
import CreateTodo from 'C:\\vue\\todo-app\\src\\components\\CreateTodo.vue'

interface Props {
  status: TodoStatus
}

const props = defineProps<Props>()

const { getTodosByStatus, deleteTodo, updateTodo } = useTodos()
const todoList = getTodosByStatus(props.status)

const groupLabel = {
  [TodoStatus.Pending]: 'Pending',
  [TodoStatus.InProgress]: 'In Progress',
  [TodoStatus.Completed]: 'Completed'
}

const onDraggableChange = (payload: any) => {
  if (payload?.added?.element) {
    updateTodo(payload?.added?.element, props.status)
  }
}
</script>

<template>
  <div class="group-wrapper">
    <h3>{{ groupLabel[props.status] }}</h3>
    <Draggable
      class="draggable"
      :list="todoList"
      group="todos"
      itemKey="id"
      @change="onDraggableChange"
    >
      <template #item="{ element: todo }">
        <li>
          {{ todo.title }}
          <span class="delete-icon" @click="deleteTodo(todo)">X</span>
          <div>
            <span class="todo-description"> {{ todo.description }}</span>
          </div>
        </li>
      </template>
    </Draggable>
    <CreateTodo :status="props.status" />
  </div>
</template>

<style scoped>
.group-wrapper {
  flex: 1;
  padding: 20px;
  background-color: rgb(46, 59, 87);
  width: 300px;
  min-width: 200px;
}

.group-wrapper li {
  list-style: none;
  background-color: aliceblue;
  color: rgb(46, 59, 87);
  padding: 2px 5px;
  cursor: grab;
  margin-bottom: 10px;
}

.draggable {
  min-height: 200px;
}

.delete-icon {
  float: right;
  cursor: pointer;
}

.todo-description {
  font-size: 12px;
  display: block;
  word-wrap: break-word;
}
</style>
