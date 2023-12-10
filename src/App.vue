<script setup lang="ts">
import { computed, ref } from "vue";
// import HelloWorld from "./components/HelloWorld.vue";

interface ListItem {
  id: number;
  label: string;
  purchased: boolean;
  priority: boolean;
}

const characterCount= computed(()=>{
  return newItem.value.length
})
const reversedItems = computed(()=> [...items.value].reverse())
const items = ref<ListItem[]>([
  // { "id": 1, "label": "10 party hats", "purchased": true, "priority": false },
  // { "id": 2, "label": "2 board games", "purchased": false, "priority": true },
  // { "id": 3, "label": "20 cups", "purchased": true, "priority": false }
])
const header = ref('Shopping List App')
const editing = ref(false)
const newItem = ref("")
const newItemHighPriority = ref(false)
const saveItem = ()=>{
  items.value.push({id: items.value.length+1, label: newItem.value, purchased: false, priority: newItemHighPriority.value})
  newItem.value = ""
  newItemHighPriority.value = false
}
const toggleEditing = (e:boolean)=>{
  editing.value = e
  newItem.value = ""
}
const togglePurchased = (item:ListItem)=> {
  item.purchased = !item.purchased
}
</script>

<template>
  <!-- <HelloWorld msg="Welcome"></HelloWorld> -->
  <div class="header">
    <h1>{{ header }}</h1>
  </div>
  <button @click="toggleEditing(false)" v-if="editing" class="btn">
    Cancel
  </button>
  <button @click="toggleEditing(true)" v-else class="btn btn-primary">
    Add Item
  </button>
  <form class="add-item-form"
    v-if="editing"
    @submit.prevent= saveItem >
    <input v-model="newItem" type="text" placeholder="Add an Item">
    <label>
      <input type="checkbox" v-model="newItemHighPriority">
      High Priority
    </label>
    <button :disabled=!newItem.length class="btn btn-primary">
      Save Item
    </button>
  </form>
  <p class="counter">
    {{ characterCount }}/200
  </p>
  <ul>
    <li v-for="item in reversedItems" 
        :key="item.id"
        @click="togglePurchased(item)"
        :class="{
          strikeout: item.purchased,
          priority: item.priority
        }"
      >
      {{ item.label }}
    </li>
  </ul>
  <p v-if="!items.length">
    No Item in List
  </p>
</template>

