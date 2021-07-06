<template>
  <div>{{ id }}</div>
</template>

<script>
export default {
  data() {
    return {
      id: ''
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      this.id = this.$route.params.id
    }
  }
}
</script>

<style>

</style>
