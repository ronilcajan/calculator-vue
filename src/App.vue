<script setup>
	import ButtonNumber from './components/ButtonNumber.vue';
	import { ref, reactive, computed } from 'vue';

	const result = ref('');

	const addNumber = (n) => {
		return result.value += n;
	}

	const clearResult = () => {
		return result.value = '';
	}

	const addProperty = (property) => {
		return result.value += property;
	}

	const calculateResult = () => {
		let expression = result.value;
		let numbersAndOperators = expression.match(/[+\-x/]|(?:\d+\.\d+)|(?:\d+\.\d+e\d+)|(?:\d+\.\d+)|(?:\d+\.\d+)|(?:\d+)/g);
		let total = parseFloat(numbersAndOperators[0]); // Initialize total with the first number

		for (let i = 1; i < numbersAndOperators.length; i++) {
			let token = numbersAndOperators[i];
			if (isNaN(token)) {
				// Token is an operator
				let nextNumber = parseFloat(numbersAndOperators[i + 1]);
				switch (token) {
					case '+':
						total += nextNumber;
						break;
					case '-':
						total -= nextNumber;
						break;
					case 'x':
						total *= nextNumber;
						break;
					case '/':
						total /= nextNumber;
						break;
					default:
						total = "Invalid operator:"+ token;
				}
				i++; // Skip the next number since we've already processed it
			} else {
				total = "Unexpected number in the expression:"+ token;
			}

			return result.value = `${total}`;
		}
	}
	
</script>

<template>

  <div class="min-h-screen bg-gray-700 flex flex-col items-center justify-center">
    <h3 class="text-3xl font-bold text-center mb-4 text-white">My Calculator</h3>

<!-- calculator goes here -->
<div class="bg-gray-800 border-2 border-gray-900 shadow-2xl rounded-lg">

  <!-- form goes here -->
  <form class="border-b-2 border-gray-900">
    <input type="text" v-model="result" class="bg-transparent p-8 rounded-t-lg outline-none focus:bg-gray-700 text-3xl text-right text-white font-mono">
  </form>

  <!-- buttons go here -->
  <div class="p-6 text-gray-800 grid grid-cols-4 gap-4 text-xl">
	<ButtonNumber @click="clearResult" class="col-span-3 bg-blue-500 hover:bg-blue-400">C</ButtonNumber>
	<ButtonNumber @click="addProperty('/')" class="bg-purple-500 hover:bg-purple-400" >&divide;</ButtonNumber>

	<ButtonNumber @click="addNumber(1)">1</ButtonNumber>
	<ButtonNumber @click="addNumber(2)">2</ButtonNumber>
	<ButtonNumber @click="addNumber(3)">3</ButtonNumber>
	<ButtonNumber @click="addProperty('x')" class="bg-purple-500 hover:bg-purple-400">&times;</ButtonNumber>

	<ButtonNumber @click="addNumber(4)">4</ButtonNumber>
	<ButtonNumber @click="addNumber(5)">5</ButtonNumber>
	<ButtonNumber @click="addNumber(6)">6</ButtonNumber>
	<ButtonNumber @click="addProperty('-')" class="bg-purple-500 hover:bg-purple-400">&minus;</ButtonNumber>

	<ButtonNumber @click="addNumber(7)">7</ButtonNumber>
	<ButtonNumber @click="addNumber(8)">8</ButtonNumber>
	<ButtonNumber @click="addNumber(9)">9</ButtonNumber>
	<ButtonNumber @click="addProperty('+')" class="bg-purple-500 hover:bg-purple-400">&plus;</ButtonNumber>

	<ButtonNumber @click="addNumber(0)" class="bg-blue-500 hover:bg-blue-400">0</ButtonNumber>
	<ButtonNumber @click="calculateResult" class="bg-purple-500 hover:bg-purple-400 col-span-3">&equals;</ButtonNumber>

  </div>
</div>

</div>
</template>