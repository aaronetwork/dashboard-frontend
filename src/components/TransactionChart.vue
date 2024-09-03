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
    transactionData: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      chartData: {
        labels: this.transactionData.map(item =>
            new Date(item.created_at).toLocaleDateString()
        ),
        datasets: [
          {
            data: this.transactionData.map(item => item.total_transaction),
            label: 'Total transaction',
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
            text: 'Total Transaction Over Time'
          }
        }
      }
    }
  }
}
</script>
