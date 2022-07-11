<template>
  <div
    class="
      container
      mx-auto
      mt-16
      max-w-2xl
      bg-gray-50
      p-12
      justify-center
      text-center
    "
  >
    <p class="text-3xl font-bold my-6">Hello There!</p>
    <p class="mb-5">
      Let's buy some insurance. It is going to take only a few steps
    </p>
    <button class="px-16 py-2 bg-slate-900 text-white rounded" @click="start">
      Start
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "HomeView",
  methods: {
    start() {
      this.$router.push("about");
    },
  },
});
</script>
