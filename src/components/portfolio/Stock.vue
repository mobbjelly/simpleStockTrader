<template>
  <div class="col-sm-6 col-md-4">
    <div class="panel panel-info">
      <div class="panel-heading">
        <div class="panel-title">
          {{stock.name}}
          <small>(Price: {{ stock.price }} | Quantity: {{stock.quantity }})</small>
        </div>
      </div>
      <div class="panel-body">
          <div class="pull-left" :class="{ 'has-error': insufficientQuantity}">
            <input type="number" class="form-control input-sm" placeholder="Quantity" v-model.number="quantity">
          </div>
          <div class="pull-right">
            <button class="btn btn-sm btn-success" @click="sellStock" :disabled="quantity <= 0 || !Number.isInteger(quantity) ||insufficientQuantity">{{ insufficientQuantity ? 'Not enough stocks' : 'Sell'}}</button>
          </div>
        </div>
    </div>
  </div>
</template>

<script>
  import { mapActions } from 'vuex'
  export default {
    props: ['stock'],
    data() {
      return {
        quantity: 0
      }
    },
    computed: {
      insufficientQuantity() {
        return this.quantity > this.stock.quantity
      },
      funds() {
        return this.$store.getters.funds
      }
    },
    methods: {
      ...mapActions({
        placeSellOrder: 'sellStock'
      }),
      sellStock() {
        const order = {
          stockId: this.stock.id,
          stockPrice: this.stock.price,
          quantity: this.quantity
        }
        this.placeSellOrder(order)
      }
    }
  }
</script>