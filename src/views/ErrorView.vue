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
    <p class="text-3xl font-bold my-6">Ooops</p>
    <p class="mb-5">
      Your age is orver our avvepted limit. <br />
      We are sorry but we canot insure you now
    </p>
    <button class="px-16 py-2 bg-slate-900 text-white rounded" @click="back">
      Ok :(
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "ErrorView",
  methods: {
    back() {
      this.$router.push("/");
    },
  },
});
</script>
