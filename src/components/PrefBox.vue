<template>
  <div>
    <input type="checkbox" @click="emit">
    {{name}}
  </div>

</template>

<script>
export default {
  name: "prefBox",
  props: {
    id: Number,
    name:String
  },
  methods:{
    emit(){
      this.$emit("pref_test",[this.id])
    }
  }
}
</script>

<style scoped>

</style>