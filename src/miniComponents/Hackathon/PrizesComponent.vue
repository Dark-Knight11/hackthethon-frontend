<template>
	<section class="text-gray-400 bg-gray-900 body-font">
		<div class="container px-5 py-24 mx-auto">
			<div class="flex flex-wrap sm:-m-4 -mx-4 -mb-10 -mt-4 md:space-y-0 space-y-6">
				<div class="flex flex-col items-center text-center w-full mb-20">
					<h2 class="text-xs text-purple-400 tracking-widest font-medium title-font mb-1">
						ROOF PARTY POLAROID
					</h2>
					<h1 class="sm:text-3xl text-2xl font-medium title-font text-white mb-2">
						Win Damn Good Prizes
					</h1>
					<button v-show="isAdmin" @click="isAddPrizeOpen = true" type="button"
						class="text-white w-fit bg-gray-800 border-0 py-1 px-3 focus:outline-none hover:bg-gray-700 rounded text-base md:mt-0">
						Add Prize
					</button>
				</div>
				<div v-for="prize in hackathon.prizes" :key="prize._id"
					class="p-4 md:w-1/3 w-full md flex flex-col text-center items-center">
					<div
						class="w-20 h-20 inline-flex items-center justify-center rounded-full bg-gray-800 text-purple-400 mb-5 flex-shrink-0">
						<svg fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round"
							stroke-width="2" class="w-10 h-10" viewBox="0 0 24 24">
							<path d="M22 12h-4l-3 9L9 3l-3 9H2"></path>
						</svg>
					</div>
					<div class="flex-grow">
						<h2 class="text-white text-lg title-font font-medium mb-3">
							{{ prize.name }}
						</h2>
						<p class="text-4xl leading-relaxed text-base">
							{{ prize.amount }}
						</p>
					</div>
				</div>
			</div>
		</div>
	</section>

	<!-- Add Prize Form -->
	<div v-show="isAddPrizeOpen"
		class="fixed h-screen w-screen bottom-0 text-gray-300 flex items-center justify-center bg-gray-700 bg-opacity-50">
		<div class="w-full max-w-sm p-6 bg-gray-900 rounded-md shadow-xl">
			<div class="flex items-center justify-between">
				<h3 class="text-2xl">Add Prize</h3>
				<svg @click="isAddPrizeOpen = false" xmlns="http://www.w3.org/2000/svg"
					class="w-8 h-8 text-red-900 cursor-pointer" fill="none" viewBox="0 0 24 24" stroke="currentColor">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
						d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" />
				</svg>
			</div>
			<div class="mt-4">
				<form class="bg-gray-800 shadow-lg rounded px-8 pt-6 pb-8 mb-4">
					<div class="identity-input mb-4">
						<label for="prize" class="block text-gray-300 text-sm text-left font-bold mb-2">
							Prize
						</label>
						<input id="prize"
							class="shadow appearance-none borderrounded w-full py-2 px-3 text-gray-900 mb-3 leading-tight focus:outline-none focus:shadow-outline"
							type="text" placeholder="Prize" v-model="prize" />
					</div>

					<div class="identity-input mb-4">
						<label for="amount" class="block text-gray-300 text-sm text-left font-bold mb-2">
							Amount
						</label>
						<input id="amount" type="number"
							class="shadow appearance-none borderrounded w-full py-2 px-3 text-gray-900 mb-3 leading-tight focus:outline-none focus:shadow-outline"
							placeholder="Amount" v-model="amount" />
					</div>

					<div class="flex items-center justify-center">
						<button @click="addPrize($event)"
							class="bg-purple-500 hover:bg-purple-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline">
							Submit
						</button>
					</div>
				</form>
			</div>
		</div>
	</div>
</template>

<script>
import axios from 'axios';
import { store, API_URL } from '@/common/user.js'
export default {
	name: 'PrizesComponent',
	props: {
		hackathon: Object,
		isAdmin: Boolean
	},
	data() {
		return {
			isAddPrizeOpen: false,
			prize: '',
			amount: null
		}
	},
	methods: {
		async addPrize(e) {
			e.preventDefault();
			const prizes = this.hackathon.prizes;
			prizes.push({
				name: this.prize,
				amount: this.amount
			});
			try {
				const res = await axios.put(`${API_URL}/hackathon/${this.hackathon._id}`, {
					"prizes": prizes
				}, {
					headers: {
						Authorization: `Bearer ${store.user.token}`
					}
				});
				console.log(res);
				this.isAddPrizeOpen = false;
				this.prize = '';
				this.amount = null;
			} catch (error) {
				console.log(error);
			}
		}
	}
}
</script>

<style></style>