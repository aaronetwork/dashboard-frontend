<template>
  <Bar id="my-chart-id" :options="chartOptions" :data="chartData"/>
</template>

<script>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

export default {
  name: 'BarChart',
  components: { Bar },
  props: {
    accountData: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      chartData: {
        labels: this.accountData.map(item =>
            new Date(item.created_at).toLocaleDateString()
        ),
        datasets: [ { data: this.accountData.map(item => item.total_account) } ]
      },
      chartOptions: {
        responsive: true
      }
    }
  }
}
</script>
