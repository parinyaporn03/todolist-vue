<script setup>
import { Button, InputText } from "primevue";
import { ref, watch } from "vue";

const { name, status, index, id } = defineProps({
  name: String,
  status: Boolean,
  index: Number,
  id: String,
});
const isEdit = ref(false);
const isDone = ref(status);
const editedName = ref(name);
const emit = defineEmits(["edit", "delete", "done"]);
watch(
  () => name,
  // callback function เมื่อ name เปลี่ยนค่า
  (newVal) => {
    editedName.value = newVal;
  }
);

watch(
  () => status,
  (newStatus) => {
    isDone.value = newStatus;
  }
);

function setEditItem() {
  if (name === editedName.value) {
    alert("somthing wrong");
  } else {
    emit("edit", {
      id: id,
      name: editedName.value,
    });
  }
  isEdit.value = false;
}

function setDone() {
  isDone.value = !isDone.value;
  emit("done", id, isDone.value);
}
</script>
<template>
  <div class="flex gap-4">
    <div class="flex w-full items-center">
      <div v-if="isEdit" class="flex gap-4 w-full items-center">
        <div>{{ index + 1 }}</div>
        <InputText
          type="text"
          v-model.trim="editedName"
          placeholder="name"
          class="w-full ring rounded px-2 py-1"
        />
      </div>
      <div v-else class="flex gap-4" :class="isDone ? 'text-slate-600' : ''">
        <div>{{ index + 1 }}</div>
        <div :class="isDone ? 'line-through ' : ''">
          {{ name }}
        </div>
      </div>
    </div>

    <!-- action -->
    <div v-if="isEdit" class="flex gap-2">
      <Button
        label="Save"
        class="cursor-pointer px-2 py-1 border rounded"
        @click="setEditItem"
      />
      <Button
        label="Cancel"
        class="cursor-pointer px-2 py-1 border rounded"
        @click="isEdit = false"
      />
    </div>
    <div v-else class="flex gap-2">
      <Button
        :label="isDone ? 'Do' : 'Done'"
        class="cursor-pointer px-2 py-1 border rounded"
        @click="setDone"
      />

      <Button
        label="Edit"
        v-if="isDone === false"
        class="cursor-pointer px-2 py-1 border rounded"
        @click="isEdit = true"
      />

      <Button
        label="Delete"
        class="cursor-pointer px-2 py-1 border rounded"
        @click="emit('delete', id)"
      />
    </div>
  </div>
</template>
