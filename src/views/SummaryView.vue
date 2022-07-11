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
    <p class="text-3xl font-bold my-6">Summary</p>
    <p class="text-2xl font-bold my-6">{{ name }}</p>
    <p class="mb-5">Name: {{ name }}</p>
    <p class="mb-5">Age: {{ age }}</p>
    <p class="mb-5">Where do you live: {{ countryList[country] }}</p>
    <p class="mb-5">Package: {{ packageTextList[packageValue] }}</p>
    <p class="mb-5">Premium: {{ totalPremium + currencyList[country] }}</p>
    <div class="flex justify-center">
      <button
        class="
          m-1
          px-10
          py-2
          bg-white
          text-slate-900
          border-2 border-gray-400
          rounded
        "
        @click="back"
      >
        Back
      </button>
      <button
        class="m-1 px-10 py-2 bg-slate-900 text-white rounded"
        @click="buy"
      >
        Buy
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import Constants from "@/constants";
import { defineComponent } from "vue";

export default defineComponent({
  name: "SummaryView",
  data() {
    return {
      countryList: Constants.countryList,
      currencyList: Constants.currencyList,
      name: this.$route.query.name as unknown as string,
      age: this.$route.query.age as unknown as number,
      country: this.$route.query.country as unknown as number,
      packageValue: this.$route.query.packageValue as unknown as number,
    };
  },
  computed: {
    premium(): number | null {
      if (this.age > 0) {
        return 10 * this.age * Constants.rateList[this.country];
      }
      return null;
    },
    totalPremium(): number | null {
      if (this.premium) {
        return (
          this.premium * (1 + Constants.packageList[this.packageValue] / 100)
        );
      }
      return null;
    },
    packageTextList(): string[] {
      if (this.premium) {
        return [
          "Standard",
          `Safe(+${(this.premium * Constants.packageList[1]) / 100}${
            Constants.currencyList[this.country]
          }, ${Constants.packageList[1]}%)`,
          `Super Safe(+${(this.premium * Constants.packageList[2]) / 100}${
            Constants.currencyList[this.country]
          }, ${Constants.packageList[2]}%)`,
        ];
      } else {
        return [
          "Standard",
          `Safe(+${Constants.currencyList[this.country]}, ${
            Constants.packageList[1]
          }%)`,
          `Super Safe(+${Constants.currencyList[this.country]}, ${
            Constants.packageList[2]
          }%)`,
        ];
      }
    },
  },
  methods: {
    back() {
      this.$router.push("/about");
    },
    buy() {
      return;
    },
  },
});
</script>
