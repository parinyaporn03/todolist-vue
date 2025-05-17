<script setup>
import { ref } from "vue";
import ListData from "./components/ListData.vue";
import { Button, InputText } from "primevue";
import { v4 as uuidv4 } from "uuid";
const item = ref("");
const list_items = ref([]);
function addItems() {
  if (item.value) {
    list_items.value.push({
      id: uuidv4(),
      name: item.value,
      status: false,
    });
    item.value = "";
  }
}

function editTask(data) {
  const task = list_items.value.find((item) => item.id === data.id);
  if (task) {
    task.name = data.name;
  }
}

function deleteTask(id) {
  const task = list_items.value.find((item) => item.id === id);
  if (!task) return;

  const isConfirmed = confirm(`คุณแน่ใจหรือไม่ว่าต้องการลบ "${task.name}"?`);
  if (isConfirmed) {
    list_items.value = list_items.value.filter((item) => item.id !== id);
  }
}

function doneTask(id, isDone) {
  const task = list_items.value.find((item) => item.id === id);
  if (task) {
    task.status = isDone;
  }
}
</script>

<template>
  <div class="flex flex-col justify-center items-center mt-5 gap-14">
    <div class="w-md flex ring p-4 gap-5 items-center rounded">
      <div class="flex flex-col gap-0.5">
        <div class="w-5 h-[3px] bg-black"></div>
        <div class="w-5 h-[3px] bg-black"></div>
        <div class="w-5 h-[3px] bg-black"></div>
      </div>
      <div class="text-xl font-bold w-full text-center">To Do List</div>
    </div>
    <!-- content -->
    <div class="flex justify-center items-center ring rounded">
      <div class="h-[550px] w-md flex flex-col gap-4 p-5">
        <!-- input  -->
        <div class="flex gap-6 w-full">
          <InputText
            type="text"
            v-model.trim="item"
            class="ring w-xl rounded"
          />
          <Button
            label="Save"
            @click="addItems"
            class="active:scale-90 cursor-pointer px-4 py-0.5 ring rounded"
          />
        </div>
        <div class="text-xl text-center" v-if="list_items.length !== 0">
          Task List
        </div>
        <!-- list -->
        <div class="h-full overflow-auto flex flex-col gap-3">
          <div v-for="(item, index) in list_items" :key="index" class="">
            <ListData
              :name="item.name"
              :status="item.status"
              :index="index"
              :id="item.id"
              @edit="editTask"
              @delete="deleteTask"
              @done="doneTask"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
