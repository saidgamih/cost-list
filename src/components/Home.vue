<template>
  <div class="py-8 lg:py-16 max-w-md mx-auto rounded px-1 flex flex-col">
    <h1 class="text-center mb-4 text-3xl uppercase font-bold text-gray-600">
      COST LIST
    </h1>
    <div class="flex flex-wrap mb-3">
      <div class="w-1/2 p-1">
        <div
          class="bg-white p-3 rounded-lg shadow h-full"
          :class="{ 'bg-red-100': balance < 0 }"
        >
          <div class="flex justify-between">
            <h4 class="text-base font-semibold uppercase text-gray-400">
              Budget
            </h4>
            <button
              v-if="!editBudget"
              @click="editBudgetStart"
              class="text-xs text-gray-500"
            >
              <i class="fas fa-pen"></i>
            </button>
          </div>
          <div>
            <form
              @submit.prevent="saveBudget"
              v-show="editBudget"
              class="flex items-center"
            >
              <input
                type="number"
                v-model="budget"
                ref="budget"
                class="text-3xl font-bold text-gray-600 w-full mr-1"
              />
              <button class="w-6 h-6 bg-blue-500 text-white rounded">
                <i class="fas fa-save"></i>
              </button>
            </form>
            <h2
              v-show="!editBudget"
              @click="editBudget = true"
              class="text-3xl font-bold text-gray-600"
            >
              {{ budget }}
            </h2>
          </div>
        </div>
      </div>
      <div class="w-1/2 p-1">
        <div class="bg-white p-3 rounded-lg shadow h-full">
          <h4 class="text-base font-semibold uppercase text-gray-400">Costs</h4>
          <h2 class="text-3xl font-bold text-gray-600">{{ cost }}</h2>
        </div>
      </div>
      <div class="w-1/2 p-1">
        <div class="bg-white p-3 rounded-lg shadow h-full">
          <h4
            class="font-bold uppercase text-sm text-gray-400 flex justify-between"
          >
            <span>Balance: </span>
            <span
              class="text-sm"
              :class="{
                'text-green-400': balance > 0,
                'text-gray-400': balance === 0,
                'text-red-400': balance < 0,
              }"
              >{{ balance }}</span
            >
          </h4>
        </div>
      </div>
      <div class="w-1/2 p-1 flex items-center justify-center">
        <button
          @click="showModal = true"
          class="bg-blue-500 hover:bg-blue-700 rounded-lg px-3 text-white w-full h-full uppercase font-semibold"
        >
          <i class="fas fa-plus"></i> New item
        </button>
      </div>
    </div>
    
    <h1 class="flex-grow uppercase font-bold text-center mb-3 tracking-widest text-gray-600">Items</h1>
    <div class="px-1"> 
      <div
        v-for="item in items"
        :key="item.id"
        class="flex items-center bg-white rounded-full shadow p-2 mt-2"
      >
        <div class="px-3 text-lg flex-grow">{{ item.title }}</div>
        <div class="px-3 text-lg font-semibold">{{ item.price }}</div>
        <button
          @click="deleteItem(item.id)"
          class="px-3 text-lg bg-red-500 rounded-full text-white h-10 w-10 flex items-center justify-center"
        >
          <i class="fas fa-trash"></i>
        </button>
      </div>
      <div
        v-if="!items.length"
        class="text-center text-gray-300 py-5 text-sm uppercase"
      >
        empty wish list
      </div>
    </div>
  </div>
  <modal v-show="showModal" :close="closeModal" :save="saveItem" />
</template>

<script>
import Modal from "./Modal.vue";
export default {
  components: {
    Modal,
  },
  data() {
    return {
      showModal: false,
      editBudget: false,
      budget: 0,
      items: [],
    };
  },
  mounted() {
    if(localStorage.getItem('budget')) {
      this.budget = localStorage.getItem('budget')
    }
    if(localStorage.getItem('items')) {
      this.items = JSON.parse(localStorage.getItem('items'))
    }
  },
  computed: {
    cost() {
      return this.items.reduce((total, item) => {
        return total + parseFloat(item.price);
      }, 0);
    },
    balance() {
      let balance = parseFloat(this.budget) - parseFloat(this.cost);
      return isNaN(balance) ? 0 : balance;
    },
  },
  methods: {
    closeModal() {
      this.showModal = false;
    },
    saveItem(title, price) {
      this.items.unshift({
        id: Math.ceil(Math.random() * 1000),
        title,
        price,
      });
      localStorage.setItem('items', JSON.stringify(this.items))
    },
    deleteItem(id) {
      this.items = this.items.filter((item) => item.id !== id);
      localStorage.setItem('items', JSON.stringify(this.items))
    },
    editBudgetStart() {
      this.editBudget = true;
      this.$refs.budget.focus();
    },
    saveBudget() {
      localStorage.setItem('budget', this.budget)
      this.editBudget = false
    }
  },
};
</script>

<style>
/*  */
</style>