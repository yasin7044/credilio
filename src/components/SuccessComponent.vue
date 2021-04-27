<template>
  <div
    :class="[
      type === 'success'
        ? 'bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded relative'
        : 'bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative',
,
    ]"
  >
    <strong class="font-bold">{{ title }} </strong>
    <p class="block sm:inline">{{ successMessage }}.</p>
  </div>
</template>

<script>
export default {
  props: {
    title: { type: String, required: "true", default: "Success" },
    successMessage: { type: String, required: "true", default: "Success" },
    type: { type: String, required: "true", default: "Success" },
  },
};
</script>

<style lang="scss" scoped>
</style>