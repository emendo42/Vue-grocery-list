<script setup>
  import { ref } from 'vue'
  import { nanoid } from 'nanoid'
  import { useStorage } from '@vueuse/core'
  import confetti from  'canvas-confetti'

  const newGrocery = ref('')
  const groceries = useStorage('groceries', [])

  const addGrocery = () =>{
    if(newGrocery.value){
      groceries.value.push({id: nanoid(), name: newGrocery.value})
      newGrocery.value=''
    } 
  }
  const deleteGrocery = id =>{
    const removeIndex = groceries.value.findIndex(grocery => grocery.id === id)
    groceries.value.splice(removeIndex, 1)
    confetti({ particleCount: 300, spread: 1000, origin: {y:1}})
  }
</script>

<template>
  <main >
    <h1 class="title">✏️ Vue Grocery List ✏️</h1>
    <form class="newGroceryForm" @click.prevent="addGrocery" >
      <input 
        id="newGrocery" 
        autocomplete="off" 
        type="text" 
        placeholder="Add an item to your list"
        v-model="newGrocery"
      />
      <button  type="submit">Add</button>
    </form>
    <h3>Pending Items: {{groceries.length}}</h3>
    <ul >
      <li v-for ="grocery in groceries" @click="deleteGrocery(grocery.id)">
        {{grocery.name}}</li>
    </ul>
  </main>
    
</template>

<style lang="postcss" scoped> 
  main {
    @apply flex flex-col justify-center items-center gap-8;

  .title {
    @apply m-2 text-6xl font-medium tracking-wider text-orange-300;
    }
  
   form {
    @apply  flex focus-within:ring-8  focus-within:ring-orange-300  focus-within:rounded-lg;
    input{
      @apply bg-gray-50 text-cyan-900 p-2 w-80 text-2xl rounded-l-md outline-none;
    }
    button {
      @apply bg-cyan-900 p-2 text-2xl font-bold rounded-r-md text-white;
      &:hover{
        @apply bg-orange-500
      }
    }
  } 
    ul{
        @apply flex flex-col items-center justify-center rounded-lg bg-cyan-900;
        li{
          @apply bg-gray-50 text-cyan-900 m-2 p-2 w-96 text-center;
          &:hover{
            @apply bg-orange-500 font-bold cursor-pointer;
          }
        }
      }
  }

</style>
