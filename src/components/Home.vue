<template>
  <div>
    <h1>Trade or View your portfolio</h1>
    <p>Your funds: {{ funds }}</p>
  </div>
</template>

<script>
  export default {
    computed: {
      funds () {
        return this.$store.getters.funds
      }
    }
  }
</script>