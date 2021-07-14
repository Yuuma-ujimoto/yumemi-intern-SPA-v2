<template>
  <div class="pref-wrapper">
    <div class="pref-box" v-for="pref in api_result" v-bind:key="pref.prefCode">
      <PrefBox :id=pref.prefCode :name=pref.prefName @pref_test=onclickCheckBox></PrefBox>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import PrefBox from "@/components/PrefBox";
import api from "./API_KEY"

export default {
  name: "PrefList",
  components: {
    PrefBox
  },
  data() {
    return {
      api_result: [],
      check_list: []
    }
  },
  mounted() {
    axios.get("https://opendata.resas-portal.go.jp/api/v1/prefectures", {
      headers: {
        "Content-Type": "application/json;charset='utf8'",
        "X-API-KEY": api.key
      }
    }).then(res => {
      this.api_result = res.data.result
    })
  },
  methods: {
    onclickCheckBox([pref_id]) {
      let check_list = this.check_list
      if (check_list.indexOf(pref_id) === -1) {
        this.check_list.push(pref_id)
      } else {
        this.check_list = check_list.filter(i => i !== pref_id)
      }
      this.$emit("ChartDataSet",[this.check_list])
    },
  }
}
</script>

<style scoped>
.pref-wrapper {
  display: flex;
  width: 100%;
  flex-wrap: wrap;
}

.pref-box {
  width: 100px;
  margin: 10px;
  border: solid 1px #2c3e50;
}
</style>