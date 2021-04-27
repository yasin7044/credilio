<template>
  <div class="h-100 w-full flex justify-center font-sans">
    
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
        <!--  {{taskArray}}
          {{taskActive}} -->
        <div
          class="flex mb-4 justify-between"
          v-for="(eachTask, index) in taskActive"
          :key="index"
        >
          <div class="">
            <!-- {{eachTask.isCompleted}} -->
            <input
              type="checkbox"
              v-model="eachTask.isCompleted"
              class="checked:bg-blue-600 checked:border-transparent mr-2"
            />
            <span
              class="text-black-700"
              :class="{
                'line-through': eachTask.isCompleted,
              }"
            >
              {{ eachTask.description }}
            </span>
          </div>
          <button
            v-if="activeClass === 'Completed'"
            class="flex-no-shrink p-1 ml-2 border-2"
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
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      activeClass: "all",
      taskArray: [],
    };
  },
  computed: {
    taskActive() {
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
      this.taskArray.push({
        id: new Date().getTime(),
        description: this.newTask,
        isCompleted: false,
      });
      this.newTask = "";
      console.log(this.taskArray);
    },
    removingTask(id) {
      const index = this.taskArray.findIndex((ele) => ele.id === id);
      this.taskArray.splice(index, 1);
    },
    removeAllTask() {
      const spliceArray = this.taskArray
        .filter((ele) => ele.isCompleted)
        .map((ele) => ele.id);
      this.taskArray = this.taskArray.filter(
        (ele) => !spliceArray.includes(ele.id)
      );
    },
  },
};
</script>

<style lang="sass" scoped>
</style>