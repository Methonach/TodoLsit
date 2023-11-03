<template>

  <div class="h-[20rem] bg-teal-500 border rounded-xl w-[80%] md:w-[60%] lg:w[40%] p-5 absolute top-10 left-1/2 -translate-x-1/2 -translate-x-1/2 bg-gradient-to-t from-indigo-500 vai-purple to-pink-500 rounded-xl px-2 py-1 active:bg-blue-500">
        <h2 class="text-lg font-bold mb-2">Todo List</h2>

        <div>

          <input type="text" placeholder="title...." class="px-5 py-1 rounded-xl bg-transparent border placeholder:text-gray-200 outline-none" v-model="TileInput">
          <button class="bg-gradient-to-t from-indigo-500 vai-purple to-pink-500 rounded-xl px-2 py-1 active:bg-blue-500 ml-5" @click="addTodo">Add</button>

        </div>

        
        <div v-for="todo in data" :key="todo">

          <div class="border px-5 py-1 rounded-xl mt-2 cursor-pointer hover:bg-gray-800 transition-all justify-between flex items-center">
          
            <div v-if="todo.doned">
              <s class="text-red-500">{{ todo.title }}</s>
            </div>
            <div v-else>
              <h1>{{ todo.title }}</h1>
            </div>

         <div>
          <button class="bg-blue-500 px-2 py-1 rounded-xl" @click="() => makeItDoned(todo.title)">Doned</button>
          <button class=" ml-2 bg-red-500 px-2 py-1 rounded-xl" @click="() => deleteTodo(todo.title)">Delete</button>
          </div>
       
          </div>  
          
      </div>
       
          
  </div>

</template>

<script>
import { ref } from 'vue'

  export default {

    setup (){
      const TileInput = ref("")
    
      
      const data = ref();

      const getAll = () => {
        data.value = JSON.parse(localStorage.getItem('todo'));
      }
      getAll();

      const addTodo = () => {
          if(!TileInput.value) return alert( "Please input something");
        const items = JSON.parse(localStorage.getItem("todo"));
        const existed = items?.find(item => item.title === TileInput.value)  
        if(existed) return alert ("Existed Todo");
        if(items) {
          localStorage.setItem('todo', JSON.stringify([...items, { title: TileInput.value, doned: false }]))
        }
        else{
          localStorage.setItem('todo', JSON.stringify([{ title: TileInput.value, doned: false }]))
        }
        TileInput.value = "";
        getAll(); 
      }
      
      const deleteTodo = (title) =>{
        const deleted = data.value.filter(item => item.title !== title);
        localStorage.setItem('todo', JSON.stringify(deleted));
        getAll();
      }
      const makeItDoned = (title) => {
        const updated = data.value.map(item => item.title === title? {...item, doned : true} : item);
        localStorage.setItem('todo', JSON.stringify(updated));
        getAll();

      } 

      return {
        data,
        TileInput,
        addTodo,
        deleteTodo,
        makeItDoned,
       
      }

    }

  }

</script>




