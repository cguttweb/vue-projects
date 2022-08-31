<script setup>
import { ref, computed } from "vue";

const header = ref("Shopping List App");
const characterCount = computed(() => {
  return newItem.value.length;
});

const editing = ref(false);

const items = ref([
  { id: 1, label: "grapes", purchased: true, highPriority: true },
  { id: 2, label: "pears", purchased: false, highPriority: false },
  { id: 3, label: "bananas", purchased: true, highPriority: false },
  { id: 4, label: "strawberry", purchased: false, highPriority: true },
]);

const reversedItems = computed(() => {
  return [...items.value].reverse();
});

const newItem = ref("");
const newItemHighPriority = ref(false);
// in script we need to access .value as where actual data is stored
const saveItem = () => {
  items.value.push({
    id: items.value.length + 1,
    label: newItem.value,
  });
  newItem.value = "";
};
const doEdit = (e) => {
  editing.value = e;
  newItem.value = "";
};

const togglePurchased = (item) => {
  item.purchased = !item.purchased;
};
</script>

<template>
  <header>
    <h2>{{ header }}</h2>
    <button
      v-if="editing"
      style="background-color: red; color: white"
      @click="doEdit(false)"
    >
      Cancel
    </button>
    <button v-else style="background-color: red; color: white" @click="doEdit(true)">
      Add Item
    </button>
  </header>
  <form v-if="editing" @submit.prevent="saveItem">
    <input type="text" placeholder="Add an item" v-model.trim="newItem" />
    <label for="">
      <input type="checkbox" v-model="newItemHighPriority" />
      High Priority
    </label>
    <button style="color: white" :disabled="newItem.length < 5">Save Item</button>

    <p class="counter">{{ characterCount }} / 200</p>
    <br />
    <ul>
      <li
        v-for="(item, index) in reversedItems"
        @click="togglePurchased(item)"
        :key="item.id"
        :class="{
          strikeout: item.purchased,
          priority: item.highPriority,
        }"
      >
        {{ item.label }}
      </li>
    </ul>
    <p v-if="!items.length">Nothing to buy!</p>
  </form>
</template>
