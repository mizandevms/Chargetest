<template>
	<section class="bg-gray-900 h-screen w-screen">
		<div class="flex flex-col justify-center items-center h-full w-full p-4">
			<form @submit.prevent="submit" class="p-4 space-y-2 bg-slate-800 border border-gray-700 text-gray-50 rounded-lg shadow-lg shadow-slate-200/20">
				<div id="div1" class="space-y-2" draggable="true" @dragstart="handleDragStart" @dragover="handleDragOver" @drop="handleDrop">
					<div id="name" draggable="true" @dragstart="handleDragStart" @dragover="handleDragOver" @drop="handleDrop" class="flex justify-start items-center space-x-2 border border-gray-500 rounded-lg py-1 sm:py-2 px-2 sm:px-4 bg-slate-700">
						<label for="name" class="font-semibold text-xs sm:text-base">Name</label>
						<input id="name" class="flex-1 rounded-r-lg px-3 py-1 text-gray-800 outline-none focus:outline-none h-full w-full" placeholder="Your name" v-model="form.name" />
					</div>
					<p v-if="msg.name == true" class="text-red-800 text-xs">Name is required, only alphabet, dot(".") and space (" ") is allowed</p>
					<div id="email" draggable="true" @dragstart="handleDragStart" @dragover="handleDragOver" @drop="handleDrop" class="flex justify-start items-center space-x-2 border border-gray-500 rounded-lg py-1 sm:py-2 px-2 sm:px-4 bg-slate-700">
						<label for="email" class="font-semibold text-xs sm:text-base">Email</label>
						<input id="email" class="flex-1 rounded-r-lg px-3 py-1 text-gray-800 outline-none focus:outline-none h-full w-full" placeholder="Your Mail" v-model="form.email" />
					</div>
					<p v-if="msg.email == true" class="text-red-800 text-xs">Email is required and must be a valid email</p>
					<div id="dob" draggable="true" @dragstart="handleDragStart" @dragover="handleDragOver" @drop="handleDrop" class="flex justify-start items-center space-x-2 border border-gray-500 rounded-lg py-1 sm:py-2 px-2 sm:px-4 bg-slate-700">
						<label for="dob" class="font-semibold text-xs sm:text-base">Date of Birth</label>
						<input type="date" id="dob" class="flex-1 rounded-r-lg px-3 py-1 text-gray-800 outline-none focus:outline-none h-full w-full" placeholder="Date of Birth" v-model="form.dob" />
					</div>
					<p v-if="msg.dob == true" class="text-red-800 text-xs">Date of birth is required</p>
					<div id="bio" draggable="true" @dragstart="handleDragStart" @dragover="handleDragOver" @drop="handleDrop" class="flex flex-col justify-start border border-gray-500 rounded-lg py-1 sm:py-2 px-2 sm:px-4 bg-slate-700">
						<label for="bio" class="font-semibold text-xs sm:text-base">Bio</label>
						<textarea v-model="form.bio" class="rounded-md px-3 py-1 text-gray-800 outline-none focus:outline-none h-full w-full" id="textarea" rows="3" placeholder="Your message"></textarea>
					</div>
					<p v-if="msg.bio == true" class="text-red-800 text-xs">Should be a textarea field with max character length of 5000 characters and display character counter</p>
					<div id="avatar" draggable="true" @dragstart="handleDragStart" class="flex items-center space-x-6 border border-gray-500 rounded-lg py-2 px-4">
						<div class="shrink-0">
							<img class="h-12 w-12 object-cover rounded-full" src="https://images.unsplash.com/photo-1580489944761-15a19d654956?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1361&q=80" alt="Current profile photo" />
						</div>
						<label class="block">
							<span class="sr-only">Choose profile photo</span>
							<input @input="form.avatar = $event.target.files[0]" type="file" class="block text-sm text-slate-500 file:mr-4 file:py-2 file:px-4 file:rounded-full file:border-0 file:text-sm file:font-semibold file:bg-gray-50 file:text-gray-500 hover:file:bg-gray-100 outline-none focus:outline-none h-full w-full" />
						</label>
					</div>
					<p v-if="msg.avatar == true" class="text-red-800 text-xs">only support "jpg", "jpeg" & "png" format, max file size is 5MB, max resolution is: 600x600</p>
				</div>
				<button type="submit" class="text-gray-50 border border-gray-500 bg-slate-700 hover:bg-opacity-60 px-4 py-2 font-extrabold w-full rounded-full">Submit</button>
			</form>
		</div>
	</section>
</template>

<style scoped></style>

<script>
import {useForm} from "@inertiajs/inertia-vue3"
import {Inertia} from "@inertiajs/inertia"
import {reactive, ref} from "vue"

export default {
	setup() {
		const form = useForm({
			name: null,
			email: null,
			dob: null,
			bio: null,
			avatar: null,
		})

		const msg = reactive({
			name: false,
			email: false,
			dob: false,
			bio: false,
			avatar: false,
		})

		function submit() {
			// Inertia.post("/users", form)

			console.log(form)

			if (form.name == null) {
				msg.name = true
			} else {
				msg.name = false
			}
			if (form.email == null) {
				msg.email = true
			} else {
				msg.email = false
			}
			if (form.dob == null) {
				msg.dob = true
			} else {
				msg.dob = false
			}

			if (form.bio == null) {
				msg.bio = true
			} else {
				msg.bio = false
			}
			if (form.avatar == null) {
				msg.avatar = true
			} else {
				msg.avatar = false
			}

			if (msg.name == false && msg.email == false && msg.dob == false && msg.bio == false && msg.avatar == false) {
				console.log("success")
			} else {
				console.log("try again")
			}
		}

		// Drag & Drop Start.
		function handleDragOver(e) {
			e.preventDefault()
		}
		function handleDragStart(e) {
			e.dataTransfer.setData("text", e.target.id)
		}
		function handleDrop(e) {
			e.preventDefault()
			var data = e.dataTransfer.getData("text")
			e.target.appendChild(document.getElementById(data))
		}
		// Drag & Drop End

		return {
			form,
			submit,
			handleDragStart,
			handleDragOver,
			handleDrop,
			msg,
		}
	},
}
</script>
