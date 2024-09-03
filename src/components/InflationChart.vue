<template>
  <Bar id="my-chart-id" :options="chartOptions" :data="chartData"/>
</template>

<script>
import {Bar} from 'vue-chartjs'
import {Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale} from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
  name: 'BarChart',
  components: {Bar},
  props: {
    inflationData: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      chartData: {
        labels: this.inflationData.map(item =>
            new Date(item.created_at).toLocaleDateString()
        ),
        datasets: [
          {
            data: this.inflationData.map(item => item.total_inflation),
            label: 'Total Inflation',
            backgroundColor: 'rgba(218,164,84,0.2)',
            borderColor: 'rgb(142,94,46)',
            borderWidth: 1
          }
        ]
      },
      chartOptions: {
        responsive: true,
        plugins: {
          title: {
            display: true,
            text: 'Total Inflation Over Time'
          }
        }
      }
    }
  }
}
</script>
