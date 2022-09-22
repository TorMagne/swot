<template>
  <main
    class="container px-4 mx-auto flex justify-center items-center flex-col"
  >
    <header class="mt-10 text-xl">
      <p>Write your comments in the white fields and hit ENTER.</p>
      <p>Add as many as you like in each box.</p>
    </header>
    <div>
      <div class="grid md:grid-cols-2 gap-4 my-10">
        <div v-for="arr in newArray" :key="arr.id">
          <header :class="[arr.color]">
            <p class="text-black p-4">{{ arr.title }}</p>
          </header>
          <div class="bg-white p-4 min-h-[10rem] min-w-[20rem]">
            <div class="flex flex-wrap gap-2">
              <span
                :class="[arr.color]"
                class="text-black py-1 px-2"
                v-for="(item, itemIndex) in arr.value"
                :key="itemIndex"
              >
                {{ item.newEntry }}
                <span
                  class="cursor-pointer"
                  @click="removeItem(arr.id, itemIndex)"
                  >x</span
                ></span
              >
            </div>
          </div>
          <div class="relative">
            <input
              type="text"
              placeholder="Type here"
              class="
                input
                w-full
                input-bordered
                rounded-none
                bg-white
                text-black
              "
              v-model="arr.inputValue"
              @keydown.enter="handleClick(arr.id, arr.inputValue)"
            />
            <button
              @click="handleClick(arr.id, arr.inputValue)"
              class="btn btn-success absolute right-0 rounded-none"
            >
              Add
            </button>
          </div>
        </div>
      </div>
    </div>
    <button class="btn btn-warning" @click="deleteAll">Clear all boxes</button>
  </main>
</template>

<script>
// import draggable from "vuedraggable";
import { useLocalStorage } from "@vueuse/core";
export default {
  // components: {
  //   draggable,
  // },
  data() {
    return {
      newArray: [],
    };
  },
  mounted() {
    this.watchLocalStorage();
  },
  methods: {
    watchLocalStorage() {
      let state = useLocalStorage("state", [
        {
          title: "Strengths",
          id: 0,
          color: "bg-amber-400",
          inputValue: "",
          value: [],
        },
        {
          title: "Weaknesses",
          id: 1,
          color: "bg-green-400",
          inputValue: "",
          value: [],
        },
        {
          title: "Oppertunities",
          id: 2,
          color: "bg-blue-400",
          inputValue: "",
          value: [],
        },
        {
          title: "Threats",
          id: 3,
          color: "bg-purple-400",
          inputValue: "",
          value: [],
        },
      ]);
      this.newArray = state;
    },

    handleClick(inputId, inputMessage) {
      if (inputMessage) {
        let newObj = { newEntry: inputMessage };
        this.newArray[inputId].value.push(newObj);
        this.newArray[inputId].inputValue = "";
      }
    },
    removeItem(inputId, itemIndex) {
      this.newArray[inputId].value.splice(itemIndex, 1);
    },
    deleteAll() {
      localStorage.clear();
      window.location.reload();
    },
  },
};
</script>
