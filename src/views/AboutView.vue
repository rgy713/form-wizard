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
    <p class="text-3xl font-bold my-6">Tell us about yourself</p>
    <div class="w-[250px] mx-auto">
      <div class="w-full mb-4">
        <p class="text-left">Name</p>
        <input
          type="text"
          class="w-full bg-white border-2 border-gray-400 mt-1 p-2 rounded"
          placeholder="Add text"
          v-model="name"
        />
      </div>
      <div class="w-full mb-4">
        <p class="text-left">Age</p>
        <input
          type="number"
          class="w-full bg-white border-2 border-gray-400 mt-1 p-2 rounded"
          placeholder="Add age"
          v-model="age"
        />
      </div>
      <div class="w-full mb-4">
        <p class="text-left">Where do you live</p>
        <select
          class="w-full bg-white border-2 border-gray-400 mt-1 p-2 rounded"
          v-model="country"
        >
          <option
            v-for="(item, index) in countryList"
            :key="index"
            :value="index"
          >
            {{ item }}
          </option>
        </select>
      </div>
      <div class="flex flex-col text-left mb-16">
        <label
          v-for="(item, index) in packageTextList"
          class="w-full"
          :key="index"
          ><input
            type="radio"
            class="mr-3"
            name="package"
            v-model="packageValue"
            :value="index"
          />{{ item }}</label
        >
      </div>
    </div>
    <p class="text-2xl font-bold my-6">
      Your premium is: {{ totalPremium }}{{ currencyList[country] }}
    </p>
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
        @click="next"
      >
        Next
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Constants from "@/constants";

export default defineComponent({
  name: "AboutView",
  data() {
    return {
      countryList: Constants.countryList,
      currencyList: Constants.currencyList,
      name: "",
      age: 50,
      country: 0,
      packageValue: 0,
    };
  },
  computed: {
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
  },
  methods: {
    back() {
      this.$router.push("/");
    },
    next() {
      if (!this.name) {
        alert("Please enter Name");
        return;
      }
      if (this.age > 0 && this.age <= 100) {
        this.$router.push({
          name: "summary",
          query: {
            name: this.name,
            age: this.age,
            country: this.country,
            packageValue: this.packageValue,
          },
        });
      } else {
        this.$router.push("error");
      }
    },
  },
});
</script>
