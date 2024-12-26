<script setup>
import { ref, watch } from "vue";

const taskName = ref("");
const prioritize = ref("");
const expiration = ref("");
const desc = ref("");

const tasks = ref([]);
const task = ref({});
const error = ref("");
const storedTasks = JSON.parse(localStorage.getItem("tasks"));
tasks.value = storedTasks ? storedTasks : [];

// const arrColor = [
//     "high-priority": {
//         "color": "#26bf94"
//     },
//     "medium-priority": {

//     },
//     "low-priority": {

//     },
// ];

const addTask = () => {
  tasks.value = JSON.parse(localStorage.getItem("tasks")) ?? [];
  task.value = {
    task_name: taskName.value,
    prioritize: prioritize.value,
    expiration: expiration.value,
    desc: desc.value,
    status: "pending",
  };

  tasks.value.push(task.value);

  localStorage.setItem("tasks", JSON.stringify(tasks.value));
  taskName.value = "";
  prioritize.value = "";
  expiration.value = "";
  desc.value = "";
};

const cancel = () => {
  taskName.value = "";
  prioritize.value = "";
  expiration.value = "";
  desc.value = "";
  error.value = "";
};

const deleteTask = (index) => {
    const isConfirm = confirm('Do you want to delete?');
    if (isConfirm) {
        tasks.value.splice(index, 1);
      
        localStorage.setItem("tasks", JSON.stringify(tasks.value));
    }
};
</script>

<template>
  <div>
    <div class="p-4 w-[600px] h-auto mx-auto bg-white shadow-xl rounded-lg">
      <h3 class="text-xl mb-2 font-semibold">To Do List</h3>
      <div class="py-3">
        <div class="mb-3">
          <input
            v-model="taskName"
            type="text"
            class="transition delay-[3000] p-2 w-full border-2 rounded-lg focus:outline-none focus:border-amber-600"
            placeholder="Task Name..."
          />
        </div>
        <div class="mb-3">
          <input
            type="date"
            v-model="expiration"
            class="transition delay-[3000] p-2 w-full border-2 rounded-lg focus:outline-none focus:border-amber-600"
          />
        </div>
        <div class="mb-3">
          <select
            name=""
            v-model="prioritize"
            id=""
            class="transition delay-[3000] p-2 w-full border-2 rounded-lg focus:outline-none focus:border-amber-600"
          >
            <option value="" selected disabled>Prioritize</option>
            <option value="high-priority">High Priority</option>
            <option value="medium-priority">Medium Priority</option>
            <option value="low-priority">Low Priority</option>
          </select>
        </div>
        <div class="mb-3">
          <textarea
            v-model="desc"
            class="transition delay-[3000] p-2 w-full border-2 rounded-lg focus:outline-none focus:border-amber-600"
            placeholder="Description your work..."
          ></textarea>
        </div>
      </div>
      <div class="flex gap-6 my-5">
        <button
          @click="addTask"
          class="transition delay-[3000] py-2 px-5 rounded-lg text-white bg-green-500 border-2 border-green-500 hover:bg-white hover:text-green-500"
        >
          Add Task
        </button>
        <button
          @click="cancel"
          class="transition py-2 px-5 rounded-lg bg-gray-100 border-2 hover:bg-gray-200"
        >
          Cancel
        </button>
      </div>
      <div class="my-3 h-[1px] bg-[#ccc]"></div>
    </div>
    <div class="my-5 w-[90%] mx-auto">
      <div class="p-3 relative overflow-x-auto shadow-md sm:rounded-lg">
        <div
          class="flex flex-column sm:flex-row flex-wrap space-y-4 sm:space-y-0 items-center justify-between pb-4"
        >
          <div>
            <button
              id="dropdownRadioButton"
              data-dropdown-toggle="dropdownRadio"
              class="transition inline-flex items-center text-white bg-[#0D92F4] border-2 border-[#0D92F4] focus:outline-none hover:bg-white focus:ring-4 hover:text-[#0D92F4] font-medium rounded-lg text-sm px-3 py-1.5 group"
              type="button"
            >
              <svg
                class="w-3 h-3 text-white group-hover:text-[#0D92F4] me-3"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                fill="currentColor"
                viewBox="0 0 20 20"
              >
                <path
                  d="M10 0a10 10 0 1 0 10 10A10.011 10.011 0 0 0 10 0Zm3.982 13.982a1 1 0 0 1-1.414 0l-3.274-3.274A1.012 1.012 0 0 1 9 10V6a1 1 0 0 1 2 0v3.586l2.982 2.982a1 1 0 0 1 0 1.414Z"
                />
              </svg>
              Add Task
            </button>
          </div>
          <label for="table-search" class="sr-only">Search</label>
          <div class="relative">
            <div
              class="absolute inset-y-0 left-0 rtl:inset-r-0 rtl:right-0 flex items-center ps-3 pointer-events-none"
            >
              <svg
                class="w-5 h-5 text-gray-500 dark:text-gray-400"
                aria-hidden="true"
                fill="currentColor"
                viewBox="0 0 20 20"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z"
                  clip-rule="evenodd"
                ></path>
              </svg>
            </div>
            <input
              type="text"
              id="table-search"
              class="block p-2 ps-10 text-sm text-gray-900 border border-gray-300 rounded-lg w-80 bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
              placeholder="Search for items"
            />
          </div>
        </div>
        <table
          class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400"
        >
          <thead
            class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400"
          >
            <tr>
              <th scope="col" class="p-4">
                <div class="flex items-center">
                  <input
                    id="checkbox-all-search"
                    type="checkbox"
                    class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded"
                  />
                  <label for="checkbox-all-search" class="sr-only"
                    >checkbox</label
                  >
                </div>
              </th>
              <th scope="col" class="px-6 py-3">Task Name</th>
              <th scope="col" class="px-6 py-3">Status</th>
              <th scope="col" class="px-6 py-3">Expiration date</th>
              <th scope="col" class="px-6 py-3">Prioritize</th>
              <th scope="col" class="px-6 py-3">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr
              class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600"
              v-for="(item,  index) in tasks"
            >
              <td class="w-4 p-4">
                <div class="flex items-center">
                  <input
                    id="checkbox-table-search-1"
                    type="checkbox"
                    class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 dark:focus:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600"
                  />
                  <label for="checkbox-table-search-1" class="sr-only"
                    >checkbox</label
                  >
                </div>
              </td>
              <th
                scope="row"
                class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white"
              >
                {{ item.task_name }}
              </th>
              <td class="px-6 py-4">{{ item.status }}</td>
              <td class="px-6 py-4">{{ item.expiration }}</td>
              <td class="px-6 py-4">
                <span class="p-2">{{ item.prioritize }}</span>
              </td>
              <td class="px-6 py-4 flex gap-2">
                <button
                  href="#"
                  class="font-medium text-blue-600 dark:text-blue-500 hover:underline"
                >
                  Edit
                </button>
                <button
                  href="#"
                  class="font-medium text-red-600 dark:text-red-500 hover:underline"
                  @click="deleteTask(index)"
                >
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<style scoped>
</style>
