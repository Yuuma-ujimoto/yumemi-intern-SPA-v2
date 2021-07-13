<template>
  <div id="app">
    <PrefList @ChartDataSet="ChartDataSet"/>
    <Chart :chartData="chartData"></Chart>

  </div>
</template>

<script>
import PrefList from "./components/PrefList";
import Chart from './components/Chart'
import axios from "axios"
import api from "./components/API_KEY"

export default {
  name: 'App',
  components: {
    Chart,
    PrefList
  },
  data() {
    return {
      chartData: {},
      pop: null,
      prefNameList:[
        '北海道', '青森県',   '岩手県', '宮城県',
        '秋田県', '山形県',   '福島県', '茨城県',
        '栃木県', '群馬県',   '埼玉県', '千葉県',
        '東京都', '神奈川県', '新潟県', '富山県',
        '石川県', '福井県',   '山梨県', '長野県',
        '岐阜県', '静岡県',   '愛知県', '三重県',
        '滋賀県', '京都府',   '大阪府', '兵庫県',
        '奈良県', '和歌山県', '鳥取県', '島根県',
        '岡山県', '広島県',   '山口県', '徳島県',
        '香川県', '愛媛県',   '高知県', '福岡県',
        '佐賀県', '長崎県',   '熊本県', '大分県',
        '宮崎県', '鹿児島県', '沖縄県'
      ]
    }
  },
  methods: {
    async ChartDataSet([check_list]) {
      let url
      let loop_count
      let population

      let result_object
      let result_list = []
      // CheckList には選択された都道府県のIDが入ってる
      let i
      for (i of check_list) {
        // itemには都道府県のID
        url = `https://opendata.resas-portal.go.jp/api/v1/population/composition/perYear?prefCode=${i}&cityCode=-`
        let res = await axios.get(url, {
          headers: {
            "Content-Type": "application/json;charset='utf8'",
            "X-API-KEY": api.key
          }
        })
        // APIでデータ取得
        loop_count = 0
        population = []
        result_object = {}
        console.log(res)
        res.data.result.data[0].data.forEach(item2 => {
          if (!(loop_count % 2)) {
            population.push(item2.value)
          }
          loop_count++
        })
        // pop = [a,b,c,d,e,f,g]
        result_object = {
          label: this.prefNameList[i-1],//近いうちにここに都道府県の名前入れること
          backgroundColor: '#f87979',
          data: population,
          fill:false
        }

        result_list.push(result_object)
      }

      this.updateData({
        labels: ["1960", "1970", "1980", "1990", "2000", "2010", "2020", "2030", "2040"],
        datasets: result_list
      })
    }
    ,
    updateData(charData) {
      console.log(charData)
      this.chartData = charData
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
