<script setup>
import { defineComponent } from "vue";
import { Icon } from "@iconify/vue";

const props = defineProps({
  todo: {
    type: Object,
    required: true,
  },
  index: {
    type: Number,
    required: true,
  },
});
defineEmits(["toggle-complete", "edit-todo", "update-todo", "delete-todo"]);
</script>

<template>
  <li>
    <input
      type="checkbox"
      :checked="todo.isCompleted"
      @input="$emit('toggle-complete', index)"
    />
    <div>
      <input
        v-if="todo.isEdited"
        class="edit-input"
        type="text"
        :value="todo.todo"
        @input="$emit('update-todo', $event.target.value, index)"
      />
      <span :class="{ 'completed-todo': todo.isCompleted }" v-else>{{
        todo.todo
      }}</span>
    </div>
    <div class="todo-actions">
      <Icon
        v-if="todo.isEdited"
        class="icon"
        icon="ph:check-circle"
        width="22"
        height="22"
        style="color: #41b080"
        @click="$emit('edit-todo', index)"
      />
      <Icon
        v-else
        class="icon"
        icon="ph:pencil-fill"
        width="22"
        height="22"
        style="color: #41b080"
        @click="$emit('edit-todo', index)"
      />
      <Icon
        class="icon"
        icon="ph:trash"
        width="22"
        height="22"
        style="color: #f95e5e"
        @click="$emit('delete-todo', todo.id)"
      />
    </div>
  </li>
</template>

<style lang="scss" scoped>
li {
  display: grid;
  grid-template-columns: 1fr 12fr 2fr;
  align-items: center;
  gap: 10px;
  padding: 16px 10px;
  background-color: #f1f1f1;
  box-shadow:
    0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);

  &:hover {
    .todo-actions {
      opacity: 1;
    }
  }

  input[type="checkbox"] {
    appearance: none;
    width: 20px;
    height: 20px;
    background-color: #fff;
    border-radius: 50%;
    box-shadow:
      0 4px 6px -1px rgb(0 0 0 / 0.1),
      0 2px 4px -2px rgb(0 0 0 / 0.1);

    &:checked {
      background-color: #41b080;
    }
  }

  .todo-actions {
    display: flex;
    gap: 6px;
    opacity: 0;
    transition: 150ms ease-in-out;
    .icon {
      cursor: pointer;
    }
  }
}

.edit-input {
  width: 100%;
  padding: 2px 6px;
  border: 2px solid #41b080;
}

.completed-todo {
  text-decoration: line-through;
}
</style>
