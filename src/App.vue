<script setup>
import {v4 as uuidv4} from "uuid"
import {ref,reactive,onBeforeMount} from "vue"

import TodoListItem from "@/components/Item.vue"

const TodoList = ref("");
const TodoLists = reactive([]);
const StorageKey = "TodoList_data";

const save_list = (key,data) =>{
    localStorage.setItem(key,JSON.stringify(data));
}

const addList=()=>{

if(TodoList.value!==""){
    const todoItem = {
        id: uuidv4(),
        content: TodoList.value,
        checkbox: false,
    }

    TodoLists.unshift(todoItem);
    save_list(StorageKey,TodoLists);

    TodoList.value="";
}
}

const removeItem = (id)=>{
    const index = TodoLists.findIndex((todoItem)=>{
        return todoItem.id ===id;
    })

    TodoLists.splice(index,1);
    save_list(StorageKey,TodoLists);
}

const checkbox_Update = (id)=>{
    const index = TodoLists.findIndex((todoItem)=>{
        return todoItem.id ===id;
    })   
    
    TodoLists[index]['checkbox']=!TodoLists[index]['checkbox']
    // console.log(TodoLists[index])
    // console.log(TodoLists)
    save_list(StorageKey,TodoLists);
}

onBeforeMount(()=>{
    if(!localStorage.getItem(StorageKey)){
    console.log('localStorage Empty Data')
    }else{
    const saveTodoLists=JSON.parse(localStorage.getItem(StorageKey));
    TodoLists.push(...saveTodoLists);

}

})

</script>

<template>
<main class="container mx-auto">
    
      <header class="m-2">
        <h1 class="text-6xl font-thin select-none">TODO!</h1>
        <div class="font-semibold select-none text-neutral-600">simple and studid todo app</div>
      </header>
      <form class="px-10 py-12 bg-white shadow-sm">
        <section class="flex">
          <input type="text" placeholder="做點重要的事吧..." class="input-todolist" v-model="TodoList" />
          <button @click.prevent="addList" class="text-xl btn-lg btn btn-neutral">新增</button>
        </section>
      </form>

      <section class="px-10 py-6 mt-4 bg-white">
        <ul class="">        
        <TodoListItem @remove-list-item ="removeItem" @checkbox-update ="checkbox_Update" v-for="l in TodoLists" :TodoList ="l" />
        </ul>
      </section>

    </main>
</template>

<style scoped>
.input-todolist{
    @apply w-full text-2xl focus:outline-none input-lg input input-bordered;
}

</style>
