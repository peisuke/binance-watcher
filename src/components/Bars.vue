<template>
  <v-container>
    <apexchart
      type="bar"
      height="3600"
      :options="chartOptions"
      :series="chartSeries"
      @dataPointSelection="dataPointSelectionHandler"
      ref="chart"
      ></apexchart>
  </v-container>
</template>

<script>
import VueApexCharts from 'vue-apexcharts'

export default {
  name: 'Bars',
  props: ['title', 'values', 'pairs', 'symbols'],
  data: () => ({
    chartData: [],
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
      dataLabels: { enabled: false },
      xaxis: { categories: []}
    },
    chartSeries: [{data: []}]
  }),
  watch: {
    values: function(){
      this.updateChart()
    }
  },
  methods: {
    updateChart: function () {
      const fundamentals = ['BTC', 'ETH', 'USD', 'USDT', 'BNB']
      let cols = this.symbols.map(x => fundamentals.includes(x) ? '#AA0000' : '#000066')
      
      let component = this.$refs.chart
      component.updateOptions( {
        title: {
          text: 'Last update: ' + this.title,
        },
        xaxis: {
          categories: this.symbols,
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
      component.updateSeries([{data: this.values}])
    },
    dataPointSelectionHandler: function (event, chartContext, config) {
      const fundamentals = ['USDT', 'USD', 'BTC', 'ETH', 'BNB']
      let arr = this.pairs[config.dataPointIndex].b
      for (var f of fundamentals) {
        var check = arr.find(value => value.match(new RegExp(f)))
        if (check !== undefined) {
          window.open('https://www.binance.com/ja/trade/' + check, '_blank')
        }
      }
    }
  },
  components: {
    apexchart: VueApexCharts
  }
}
</script>
