<template>
  <Line id="my-chart-id" :options="chartOptions" :data="chartData"/>
</template>

<script>
import { Line } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, LineElement, PointElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, LineElement, PointElement, CategoryScale, LinearScale)

export default {
  name: 'LineChart',
  components: { Line },
  props: {
    stakingPoolData: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      chartData: {
        labels: this.stakingPoolData.map(item =>
            new Date(item.created_at).toLocaleDateString()
        ),
        datasets: [
          {
            label: 'Bonded',
            data: this.stakingPoolData.map(item => item.total_bonded),
            borderColor: 'rgb(75, 192, 192)',
            backgroundColor: 'rgba(75, 192, 192, 0.2)',
            borderWidth: 2,
            tension: 0.4,
            fill: true,
          },
          {
            label: 'Unbonded',
            data: this.stakingPoolData.map(item => item.total_unbonding),
            borderColor: 'rgb(255, 99, 132)',
            backgroundColor: 'rgba(255, 99, 132, 0.2)',
            borderWidth: 2,
            tension: 0.4,
            fill: true,
          }
        ]
      },
      chartOptions: {
        responsive: true,
        plugins: {
          title: {
            display: true,
            text: 'Staking pool'
          }
        },
        scales: {
          x: {
            title: {
              display: true,
            }
          },
          y: {
            title: {
              display: true,
            }
          }
        }
      }
    }
  }
}
</script>
