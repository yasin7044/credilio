<template>
  <div>
    <div class="w-full flex justify-center font-sans">
      <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg">
        <div class="mb-4">
          <h1 class="font-bold">#todo</h1>
          <div class="flex mt-4 justify-between">
            <div
              @click="selectedActiveTab('all')"
              :class="[
                activeClass === 'all' ? 'underline' : '',
                activeClass === 'all' ? 'text-blue-500' : '',
              ]"
            >
              All
            </div>
            <div
              @click="selectedActiveTab('Active')"
              :class="[
                activeClass === 'Active' ? 'underline' : '',
                activeClass === 'Active' ? 'text-blue-500' : '',
              ]"
            >
              Active
            </div>
            <div
              @click="selectedActiveTab('Completed')"
              :class="[
                activeClass === 'Completed' ? 'underline' : '',
                activeClass === 'Completed' ? 'text-blue-500' : '',
              ]"
            >
              Completed
            </div>
          </div>
          <div class="flex mt-4">
            <input
              class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker"
              placeholder="Add Todo"
              v-model="newTask"
              @keyup.enter="addNewTask()"
            />
            <button
              class="flex-no-shrink p-2 w-20 bg-blue-600 text-white rounded"
              @click.enter="addNewTask()"
            >
              Add
            </button>
          </div>
        </div>
        <div>
          <div
            class="flex mb-4 justify-between items-center flex-1 flex-wrap"
            v-for="eachTask in taskActive"
            :key="eachTask.id"
          >
            <div style="flex: 3" class="flex items-baseline flex-shrink">
              <input
                v-model="eachTask.isCompleted"
                class="checked:bg-blue-600 checked:border-transparent mr-2"
                type="checkbox"
              />
              <p
                class="text-black-700 text-left break-all leading-4"
                :class="{
                  'line-through': eachTask.isCompleted,
                }"
              >
                {{ eachTask.description }}
              </p>
            </div>
            <button
              class="flex-no-shrink p-1 ml-2 border-2"
              style="flex: 0"
              v-if="activeClass === 'Completed'"
              @click="removingTask(eachTask.id)"
            >
              Remove
            </button>
          </div>
          <button
            v-if="activeClass === 'Completed' && taskActive.length"
            class="flex-no-shrink p-1 ml-2 border-2 float-right"
            @click="removeAllTask()"
          >
            Remove All
          </button>
        </div>
      </div>
    </div>
    <div
      class="absolute bottom-0 right-0 h-16 w-auto px-4 my-20"
      v-if="showAlert"
    >
      <success-component
        :title="title"
        :successMessage="successMessage"
        :type="type"
      ></success-component>
    </div>
  </div>
</template>

<script>
import SuccessComponent from "./SuccessComponent.vue";
export default {
  components: {
    SuccessComponent,
  },
  data() {
    return {
      newTask: "",
      activeClass: "all",
      taskArray: [],
      showAlert: false,
      title: "",
      successMessage: "",
      type: "",
    };
  },
  computed: {
    taskActive() {
      console.log('jo')
      if (this.activeClass === "all") {
        return this.taskArray;
      } else if (this.activeClass === "Active") {
        return this.taskArray.filter((ele) => !ele.isCompleted);
      } else if (this.activeClass === "Completed") {
        return this.taskArray.filter((ele) => ele.isCompleted);
      }

      return [];
    },
  },
  methods: {
    selectedActiveTab(tab) {
      this.activeClass = tab;
    },
    addNewTask() {
      if (!this.newTask) {
        this.showAlertMessage(
          "Error!!",
          "Please enter task notes",
          "error",
          "3000"
        );
      } else {
        this.taskArray.push({
          id: new Date().getTime(),
          description: this.newTask,
          isCompleted: false,
        });
        this.newTask = "";
        this.showAlertMessage(
          "Created!!",
          "New Task Added Successfully",
          "success",
          "3000"
        );
      }
    },
    removingTask(id) {
      const index = this.taskArray.findIndex((ele) => ele.id === id);
      this.taskArray.splice(index, 1);
      this.showAlertMessage(
        "Remove!!",
        "Single Item remove Successfully",
        "danger",
        "3000"
      );
    },
    removeAllTask() {
      const spliceArray = this.taskArray
        .filter((ele) => ele.isCompleted)
        .map((ele) => ele.id);
      this.taskArray = this.taskArray.filter(
        (ele) => !spliceArray.includes(ele.id)
      );
      this.showAlertMessage(
        "Remove!!",
        "All Complete Item Remove Successfully",
        "danger",
        "3000"
      );
    },
    showAlertMessage(title, successMessage, type, time = 3000) {
      this.showAlert = true;
      this.title = title;
      this.successMessage = successMessage;
      this.type = type;
      setTimeout(() => {
        this.showAlert = false;
      }, time);
    },
  },
};
</script>

<style lang="sass" scoped>
</style>