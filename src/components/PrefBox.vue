<template>
  <div>
    <input type="checkbox" @click="test">
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
    test(){
      console.log(this.id)
      this.$emit("pref_test",[this.id])
    }
  }
}
</script>

<style scoped>

</style>