<template>
	<div class="col-sm-6 col-md-4">
		<div class="panel panel-success">
			<div class="panel-heading">
				<h4 class="title">
					{{ stock.name }}
					<small>(Price: {{ stock.price }})</small>
				</h4>
			</div>
			<div class="panel-body">
				<div class="pull-left">
					<input type="Number" class="form-control" placeholder="Quantity" v-model="quantity">
				</div>
				<div class="pull-right">
					<button
						class="btn btn-success"
						@click="buyStock"
						:disabled="quantity <= 0 || !isInt(quantity)"
					>Buy</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	props: ["stock"],
	data() {
		return {
			quantity: 0
		};
	},
	methods: {
		buyStock() {
			const order = {
				stockId: this.stock.id,
				stockPrice: this.stock.price,
				quantity: this.quantity
			};
			if (order.quantity * order.stockPrice > this.$store.getters.funds) {
				alert('You have Insufficient funds');
				return
			};
			// console.log(order);
			this.$store.dispatch("buyStock", order);
			this.quantity = 0;
		},
		isInt(n) {
			return n % 1 === 0;
		}
	}
};
</script>
