  <template>
    <div id="app" class="container mx-auto px-4 py-8 bg-gray-100">
      <h1 v-if="isLoggedIn" class="text-3xl font-bold text-center mb-8">Your Lists</h1>
      <div v-else class="flex my-auto items-center justify-center">
        <p class="text-2xl text-red-500 font-bold">You are not logged in. Please <a href="/auth/login" class="text-blue-500 underline">login</a> first.</p>
      </div>
  
      <template v-if="isLoggedIn">
        <button @click="addList" class="w-[40%] flex justify-center mx-auto bg-green-500 hover:bg-green-700 text-white font-semibold my-3 py-2 rounded">Add New List</button>
        <div v-for="list in userLists" :key="list._id" class="mb-8 bg-white rounded shadow-md p-4">
          <div class="flex justify-between items-center">
            <h2 class="text-2xl mb-3 font-semibold cursor-pointer">
              {{ list.title }}
            </h2>
            <div class="flex space-x-2">
              <button @click="editListName(list._id)" class="text-blue-500 hover:text-blue-700 focus:outline-none">
                Edit
              </button>
              <button @click="removeList(list._id)" class="text-red-500 hover:text-red-700 focus:outline-none">
                Remove
              </button>
            </div>
          </div>
          <ul class="list-none ml-4">
            <li v-for="(task, index) in list.tasks" :key="task._id" class="flex items-center mb-2 w-[85%]">
              <input type="checkbox" v-model="task.completed" @change="updateTaskCompletion(list._id, index)" class="mr-2 focus:ring-blue-500 focus:ring-opacity-50">
              <span v-if="!task.completed" class="text-gray-700 pl-2">{{ task.description }}</span>
              <span v-else class="text-gray-400 line-through pl-2">{{ task.description }}</span>
              <div class="flex justify-end ml-auto">
                <button @click="removeTask(list._id, index)" class="text-red-500 hover:text-red-700 focus:outline-none">
                  Remove
                </button>
              </div>
            </li>
            <li class="flex items-center">
              <input v-model="newTasks[list._id]" type="text" class="border border-gray-300 rounded px-2 py-1 w-full focus:ring-blue-500 focus:ring-opacity-50" placeholder="Add new task">
              <button @click="addTask(list._id)" class="ml-2 bg-blue-500 hover:bg-blue-700 text-white font-semibold py-1 px-2 rounded">Add</button>
            </li>
          </ul>
        </div>
      </template>
    </div>
  </template>
  
  
  
  <script setup>
    import { ref, onMounted } from 'vue';
    import axios from 'axios';
    import {url} from '../url.js'
  
    const userLists = ref([]);
    const isLoggedIn = ref(false);
    const userData = ref(null);
    const newTasks = ref({});
  
    const getUser = async() => {
      try {
        const response = await axios.get(`${url}/api/auth/refresh`, { withCredentials: true });
        console.log("response",response.data);
        userData.value = response.data;
        // console.log('User data:', userData.value);
        if(userData.value){
          isLoggedIn.value = true;
        }
      } catch (error) {
        console.error('Error fetching user data:', error);
      }
    }
  
    onMounted(async () => {
      await getUser();
      await fetchUserLists();
    });
  
    async function fetchUserLists() {
      try {
        console.log("userData",userData.value._id);
        const response = await axios.get(`${url}/api/list/${userData.value._id}`,{ withCredentials: true });
        console.log("All Lists Fetched",response.data.data);
        userLists.value = response.data.data;
        
      } 
      catch (error) {
        console.error('Error fetching todo lists:', error);
      }
    }
  
    async function addList() {
      const listName = prompt("Enter the name of the todo list:");
      if(!listName){
        return;
      }
      try {
        const response = await axios.post(`${url}/api/list`,{ title:listName, userId:userData.value._id },{ withCredentials: true });
        console.log("Current List Added",response.data.data);
        await fetchUserLists();
      } 
      catch (error) {
        console.error('Error creating todo:', error);
      }
    }
  
    async function editListName(listId){
      const newName = prompt("Enter the new name for the todo list:");
      if (newName) {
        try {
          const response = await axios.put(`${url}/api/list/${listId}`,{ title:newName },{ withCredentials: true });
          console.log("List Name Updated",response.data.data);
          await fetchUserLists();
        } 
        catch (error) {
          console.error('Error updating todo name:', error);
        }
      }
    }
  
    async function removeList(listId) {
      try {
        const response = await axios.delete(`${url}/api/list/${listId}`,{ withCredentials: true });
        console.log("List Deleted",response.data.data);
        await fetchUserLists();
      } 
      catch (error) {
        console.error('Error deleting todo:', error);
      }
    }
  
    async function addTask(listId) {
      const newTask = newTasks.value[listId];
      if (!newTask) {
        return;
      }
      try {
        const response = await axios.post(`${url}/api/task`,{ description:newTask, listId },{ withCredentials: true });
        console.log("Task Added",response.data.data);
        await fetchUserLists();
      } 
      catch (error) {
        console.error('Error adding task:', error);
      }
      newTasks.value[listId] = '';
    }
  
    async function updateTaskCompletion(listId, index){
      console.log(listId,index);
      try {
        const response = await axios.put(`${url}/api/task/toggle`,{listId,index} ,{ withCredentials: true });
        console.log("Task Updated",response.data.data);
        await fetchUserLists();
      } 
      catch (error) {
        console.error('Error updating task:', error);
      }
    }
  
    async function removeTask(listId,index){
      try {
        const response = await axios.delete(`${url}/api/task/${listId}/${index}`,{ withCredentials: true });
        console.log("Task Deleted",response.data.data);
        await fetchUserLists();
      } 
      catch (error) {
        console.error('Error deleting task:', error);
      }
    }
  
  </script>