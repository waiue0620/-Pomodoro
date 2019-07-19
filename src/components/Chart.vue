<script>
import { Bar, mixins } from 'vue-chartjs'
const { reactiveData } = mixins

export default {
  extends: Bar,
  mixins: [reactiveData],
  data: () => ({
    chartdata: {
      labels: ['7/1', '7/2', '7/3', '7/4', '7/5', '7/6', '7/7'],
      datasets: [
        {
          label: 'daar',
          backgroundColor: ['#fff', '#fff', '#fff', '#fff', '#fff', '#fff', '#FF4384'],
          data: [16, 12, 16, 8, 12, 4, 20]
        }
      ]
    },
    options: {
      maintainAspectRatio: false,
      scales: {
        xAxes: [{
          categoryPercentage: 0.9, // 宽度设置属性
          barPercentage: 0.6, // 宽度设置属性
          ticks: {
            fontColor: '#fff',
            fontSize: 15
          },
          gridLines: {
            display: false,
            lineWidth: 1,
            color: '#fff'
          }
        }],
        yAxes: [{
          ticks: {
            beginAtZero: true,
            fontColor: '#fff',
            fontSize: 15
          },
          gridLines: {
            display: false,
            lineWidth: 1,
            color: '#fff'
          }
        }]
      },
      legend: {
        display: false
      }
    }
  }),
  mounted () {
    this.renderChart(this.chartdata, this.options)
  }
}

</script>
