<template>
  <div class="w-screen h-screen bg-black bg-opacity-50 fixed top-0 left-0 flex items-center justify-center px-2">
      <div class="w-full max-w-md bg-white p-4 rounded relative">
          <button @click="closeModal" class="absolute right-1 top-1 px-2 text-gray-400 hover:text-gray-600">
              <i class="fas fa-times"></i>
          </button>
          <h2 class="text-lg uppercase font-bold text-gray-700 mb-3">New item</h2>
          <form @submit.prevent="saveItem">
              <div class="flex flex-col mb-4">
                  <label for="title" class="uppercase font-semibold text-sm mb-2 text-gray-500">Item name</label>
                  <input type="text" id="title" v-model="title" class="border text-xl px-2 py-1 rounded-lg">
              </div>
          
              <div class="flex flex-col mb-4">
                  <label for="price" class="uppercase font-semibold text-sm mb-2 text-gray-500">Item price</label>
                  <input type="number" id="price" v-model="price" class="border text-xl px-2 py-1 rounded-lg">
              </div>
              <div class="flex justify-end">
                  <button @click="closeModal" type="button" class="focus:outline-none mx-1 px-2 py-1 text-gray-500 uppercase text-sm">Cancel</button>
                  <button type="submit" class="focus:outline-none mx-1 bg-blue-500 text-white px-2 py-1 uppercase font-semibold rounded-lg"><i class="fas fa-plus"></i> Add</button>
              </div>
          </form>
      </div>
  </div>
</template>

<script>
export default {
    props: {
        close: Function,
        save: Function
    },
    data() {
        return {
            title: "",
            price: 0
        }
    },
    methods: {
        closeModal() {
            this.close(),
            this.title = "";
            this.price = 0;
        },
        saveItem() {
            this.save(this.title, this.price);
            this.closeModal();
        }
    }
}
</script>

<style>

</style>