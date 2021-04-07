<template>
  <v-container>
    <apexchart
      type="bar"
      height="3600"
      :options="chartOptions"
      :series="chartSeries"
      ref="chart"
      ></apexchart>
  </v-container>
</template>

<script>
import axios from 'axios'
import VueApexCharts from 'vue-apexcharts'

export default {
  name: 'Bars',
  data: () => ({
    chartOptions: {
      plotOptions: {
        bar: {
          horizontal: true,
          barHeight: '90%'
        },
      },
      title: {
        text: 'Coin',
        align: 'center',
      },
      dataLabels: {
        enabled: false
      },
      xaxis: {
        categories: [],
      },
      //tooltip: {
      //  custom: function() {
      //    return (
      //      '<div>aaa</div>'
      //    )
      //  }
      //}
    },
    chartSeries: [
      {
        data: []
      }
    ]
  }),
  mounted () {
    let url = 'https://lbupa7i8jf.execute-api.us-west-2.amazonaws.com/list/GetBinanceProfitSupremeMeterSystem'
    axios.get(url).then(ret => {
      const fundamentals = ['BTC', 'ETH', 'USD', 'USDT']
      
      let last_modified = ret.data.last_modified
      let symbols = ret.data.data.map(x => x.symbol)
      let values = ret.data.data.map(x => x.val)
      let cols = ret.data.data.map(x => fundamentals.includes(x.symbol) ? '#AA0000' : '#000066')

      let component = this.$refs.chart
      component.updateOptions( {
        title: {
          text: 'Last update: ' + last_modified,
          align: 'center',
        },
        xaxis: {
          categories: symbols,
          labels: {
            style: {
              colors:['#000000']
            }
          }
        },
        yaxis: {
          labels: {
            style: {
              colors: cols
            }
          }
        },
        colors: [function({ value }) {
          if (value < 0.2) {
              return '#F9581C'
          } else if (value < 0.4) {
              return '#F9AF1C'
          } else if (value < 0.6) {
              return '#F9E81C'
          } else if (value < 0.8) {
              return '#D4F91C'
          } else {
              return '#50F926'
          }
        }, function() {
          return '#000000'
        }] 
      });
      component.updateSeries([
        {
          data: values
        }
      ])
    })
  },
  components: {
    apexchart: VueApexCharts
  }
}
</script>
