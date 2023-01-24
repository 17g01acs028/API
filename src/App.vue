<script setup>
import { ref, onMounted, computed, watch } from 'vue'
import axios from 'axios'

const users = ref([])
const name = ref('')

const input_content = ref('')
const input_id = ref('')
const input_lname = ref('')
const input_email = ref('')
const input_job = ref('')

//https://reqres.in/api/users

const users_asc = computed(() => users.value.sort((a,b) =>{
	return a.createdAt - b.createdAt
}))

watch(name, (newVal) => {
	localStorage.setItem('name', newVal)
})

watch(users, (newVal) => {
	localStorage.setItem('users', JSON.stringify(newVal))
}, {
	deep: true
})


//add New User
const addUser = () => {
	if (input_content.value.trim() === '' ) {
		return
	}else if(input_content.value.trim() != '' ){
		// if ID has Value Upadate That Data
		axios.put('https://reqres.in/api/users/'+id,
	{
		content: input_content.value,
		done: false,
		email:input_email.value,
		lader:input_job.value,
		lname:input_lname,
		editable: false,
		createdAt: new Date().getTime()
	}
	)
      .then(response => {
		console.log(response)
      }).catch(error=>console.log(error))
	}



	users.value.push({
		content: input_content.value,
		done: false,
		email:input_email.value,
		lader:input_job.value,
		lname:input_lname,
		id:Math.floor(Math.random() * (1000 - 100) + 100),
		editable: false,
		createdAt: new Date().getTime()
	})
  // Fetch Data From Api
	axios.post('https://reqres.in/api/users',
	{
		content: input_content.value,
		done: false,
		email:input_email.value,
		lader:input_job.value,
		lname:input_lname,
		id:Math.floor(Math.random() * (1000 - 100) + 100),
		editable: false,
		createdAt: new Date().getTime()
	}
	)
      .then(response => {
		console.log(response)
      }).catch(error=>console.log(error))
}

const removeUser = (user) => {
	users.value = users.value.filter((t) => t !== user)
	axios.delete('https://reqres.in/api/users/'+id)
      .then(response => {
		console.log(response)
      }).catch(error=>console.log(error))
}

const fetchUser = () => {
      axios.get('https://reqres.in/api/users')
      .then(response => {
		users.value=response.data.data;
      })
    } 

	const updateUser= function(user) {
		input_id=user;

    } 

onMounted(() => {
	name.value = localStorage.getItem('name') || ''
	//users.value = JSON.parse(localStorage.getItem('users')) || []
	fetchUser()
})
</script>

<template>
	<main class="app">
		
		<section class="greeting">
			<h2 class="title">
				Hello, <input type="text" id="name" placeholder="This is User List" v-model="name">
			</h2>
		</section>

		<section class="create-todo">

			<form id="new-todo-form" @submit.prevent="addUser">
				<h4>ID</h4>
				<input 
					type="text" 
					name="id" 
					readonly
					id="id" 
					v-model="input_id" />

				<h4>Enter First Name</h4>
				<input 
					type="text" 
					name="lname" 
					id="lname" 
					placeholder="e.g. steve"
					v-model="input_lname" />
					
					<h4>Job</h4>
				<input 
					type="text" 
					name="lader" 
					id="lader" 
					placeholder="e.g. steve"
					v-model="input_job" />

					<h4>Enter Last Name</h4>
				<input 
					type="text" 
					name="content" 
					id="content" 
					placeholder="e.g. Mark"
					v-model="input_content" />

					<h4>Email</h4>
					<input 
					type="email" 
					name="email" 
					id="email" 
					placeholder="e.g. steve@gmail.com"
					v-model="input_email" />

				
				<input type="submit" value="Add User" />
			</form>
		</section>

		<section class="todo-list">
			
			<div class="list" id="todo-list">

				
				<section class="table__header">
            <h1>User List</h1>
            
        </section>
				<section class="table__body">
            <table>
                <thead>
                    <tr>
                        <th> Id <span class="icon-arrow">&UpArrow;</span></th>
                        <th> User <span class="icon-arrow">&UpArrow;</span></th>
                        <th> Email <span class="icon-arrow">&UpArrow;</span></th>
                        <th> Status <span class="icon-arrow">&UpArrow;</span></th>
                        
                    </tr>
                </thead>
                <tbody >
                    <tr v-for="user in users_asc" >
                        <td> <input readonly type="text" v-model="user.id" /> </td>
                        <td> <img :src="user.avatar" alt=""><input readonly type="text" v-model="user.content" /> <input readonly type="text" v-model="user.lname" /></td>
                        <td> <input readonly type="text" v-model="user.email" /></td>
                        <td>
                            <div class="actions">
							 	<button class="delete" @click="removeUser(user)">Delete</button>
						    </div>
							<div class="actions">
							 	<button class="update" @click="updateUser(user.id)">Click to Update</button>
						    </div>
                        </td>
                        
                    </tr>
                  
                   
                </tbody>
            </table>
        </section>
			</div>
		</section>

	</main>
</template>
