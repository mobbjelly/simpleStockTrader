<template>
  <div class="col-sm-6 col-md-4">
    <div class="panel panel-success">
      <div class="panel-heading">
        <div class="panel-title">
          {{stock.name}}
          <small>(Price: {{ stock.price }})</small>
        </div>
      </div>
      <div class="panel-body">
          <div class="pull-left" :class="{ 'has-error': insufficientFunds}">
            <input type="number" class="form-control input-sm" placeholder="Quantity" v-model.number="quantity">
          </div>
          <div class="pull-right">
            <button class="btn btn-sm btn-success" @click="buyStock" :disabled="quantity <= 0 || !Number.isInteger(quantity) || insufficientFunds">{{ insufficientFunds ? 'insufficient Funds' : 'buy'}}</button>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['stock'],
    data() {
      return {
        quantity: 0
      }
    },
    computed: {
      funds() {
        return this.$store.getters.funds
      },
      insufficientFunds() {
        return (this.stock.price * this.quantity) > this.funds
      }
    },
    methods: {
      buyStock() {
        if (this.stock.price * this.quantity > this.funds) {
          alert("你钱不够！")
          return
        }
        const order = {
          stockId: this.stock.id,
          stockPrice: this.stock.price,
          quantity: this.quantity
        }
        console.log(order)
        this.$store.dispatch('buyStock', order)
        this.quantity = 0
      }
    }
  }
</script>