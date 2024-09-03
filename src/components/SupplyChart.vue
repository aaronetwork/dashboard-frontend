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
    supplyData: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      chartData: {
        labels: this.supplyData.map(item =>
            new Date(item.created_at).toLocaleDateString()
        ),
        datasets: [
          {
            data: this.supplyData.map(item => item.total_supply),
            label: 'Total Supply',
            backgroundColor: 'rgba(186,72,170,0.2)',
            borderColor: 'rgb(159,51,129)',
            borderWidth: 1
          }
        ]
      },
      chartOptions: {
        responsive: true,
        plugins: {
          title: {
            display: true,
            text: 'Total Supply Over Time'
          }
        }
      }
    }
  }
}
</script>
