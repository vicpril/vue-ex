<template>
	<div class="col-sm-6 col-md-4">
		<div class="panel panel-info">
			<div class="panel-heading">
				<h4 class="title">
					{{ stock.name }}
					<small>(Price: {{ stock.price }} | Quantity: {{ stock.quantity }})</small>
				</h4>
			</div>
			<div class="panel-body">
				<div class="pull-left">
					<input
						type="number"
						class="form-control"
						placeholder="Quantity"
						v-model="quantity"
						:class="{danger: insufficientQuantity}"
					>
				</div>
				<div class="pull-right">
					<button
						class="btn btn-info"
						@click="sellStock"
						:disabled="insufficientQuantity || quantity <= 0 || !isInt(quantity)"
					>Sell</button>
				</div>
			</div>
		</div>
	</div>
</template>


<style scoped>
.danger {
	border: 1px solid red;
}
</style>

<script>
import { mapActions } from "vuex";

export default {
	props: ["stock"],
	data() {
		return {
			quantity: 0
		};
	},
	computed: {
		insufficientQuantity() {
			return this.quantity > this.stock.quantity;
		}
	},
	methods: {
		...mapActions({
			placeSellOrder: "sellStock"
		}),
		sellStock() {
			const order = {
				stockId: this.stock.id,
				stockPrice: this.stock.price,
				quantity: this.quantity
			};
			this.placeSellOrder(order);
			this.quantity = 0;
		},
		isInt(n) {
			return n % 1 === 0;
		}
	}
};
</script>
