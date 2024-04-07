<template>
	<h1>Seive of Eratosthenes</h1>
	Hello
	<div class="setup">
		<div class="block" v-for="n in numbers" :key="n" :ref="n">{{ n }}</div>
	</div>
	<hr />
	<div>
		<h2>Primes:</h2>
		<span v-for="n in primes">{{ n }},</span>
	</div>
</template>

<script>
export default {
	data() {
		return {
			limit: 100,
			numbers: Array.from({ length: 100 }, (_, index) => index + 1),
			timeGap: 150,
			counter: 0,
			primes: [],
		};
	},
	created() {
		console.clear();
	},
	mounted() {
		this.makePrimes();
	},
	methods: {
		makePrimes() {
			let composites = [];
			let primes = [];
			let i = 2;
			while (i <= this.limit) {
				if (composites.includes(i)) {
					continue;
				}

				// Its a prime
				primes.push(i);
				this.delayed(i, `hsl(${(i * 10) % 360}, 80%, 50%, .7)`, "prime");

				// mark its multiples as composite
				for (let j = i * i; j <= this.limit; j = j + i) {
					if (composites.includes(j)) {
						// if already marked composite, don't change its color
						continue;
					}
					composites.push(j);
					this.delayed(j, `hsl(${(i * 10) % 360}, 80%, 50%, .3)`, "composite");
				}

				// check for the next prime
				i = i + 1;
				console.log("Now checking " + i);
			}
		},
		delayed(el, color, type) {
			this.counter += 1;
			setTimeout(() => {
				this.$refs[el][0].style.backgroundColor = color;
				if (type === "prime") this.primes.push(el);
			}, this.timeGap * this.counter);
		},
	},
};
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
	text-transform: uppercase;
}

.container {
	width: 80%;
	height: 300px;
	border: 1px solid;
	margin: 80px auto 20px;
	border-radius: 8px;
}

body {
	padding: 30px;
	max-width: 600px;
	margin: auto;
}

::selection {
	user-select: none;
}

.block {
	display: inline-grid;
	place-content: center;
	width: 40px;
	text-align: center;
	aspect-ratio: 1;
	border-radius: 4px;
	border: 1px solid #ddd;
	margin: 2px;
}
</style>
