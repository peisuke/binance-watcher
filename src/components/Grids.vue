<template>
  <v-container>
    <apexchart
      type="heatmap"
      height="3600"
      :options="chartOptions"
      :series="chartSeries"
      ref="chart"
      :style="styles"
      ></apexchart>
  </v-container>
</template>

<script>
import VueApexCharts from 'vue-apexcharts'

export default {
  name: 'Bars',
  props: ['title', 'historical', 'pairs', 'symbols'],
  data: () => ({
    chartData: [],
    chartOptions: {
      plotOptions: {
        heatmap: {
          radius: 2,
          useFillColorAsStroke: false,
          enableShades: false,
          colorScale: {
            ranges: [{
                from: 0,
                to: 0.2,
                color: '#F9581C'
              },
              {
                from: 0.2,
                to: 0.4,
                color: '#F9AF1C'
              },
              {
                from: 0.4,
                to: 0.6,
                color: '#F9E81C'
              },
              {
                from: 0.6,
                to: 0.8,
                color: '#D4F91C'
              },
              {
                from: 0.8,
                to: 1,
                color: '#50F926'
              }
            ]
          }
        }
      },
      title: {
        text: 'Coin',
        align: 'center',
      },
      yaxis: {
        reversed: true,
        labels: {
        }
      },
      grid: {
        padding: {
          right: 20
        }
      },
      dataLabels: { enabled: false }
    },
    chartSeries: [],
    translateX: '46px',
    translateY: '0px'
  }),
  watch: {
    historical : function(){
      this.updateChart()
    }
  },
  computed: {
    styles () {
      return {
        '--translateX': this.translateX,
        '--translateY': this.translateY 
      }
    }
  },
  methods: {
    updateChart: function () {
      //const fundamentals = ['BTC', 'ETH', 'USD', 'USDT', 'BNB']
      //let cols = this.symbols.map(x => fundamentals.includes(x) ? '#AA0000' : '#000066')
      
      let component = this.$refs.chart
      component.updateOptions( {
        title: {
          text: 'Last update: ' + this.title,
        },
        //yaxis: {
        //  labels: {
        //    style: {
        //      colors: cols
        //    }
        //  }
        //}
      })
      component.updateSeries(this.historical)
      //this.translateX = component.chart.w.globals.translateYAxisX[0] + 'px'
      this.translateY = -8 - (this.symbols.length - 100) / 10 + 'px'
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

<style>
.apexcharts-yaxis {
  transform: translate(var(--translateX), var(--translateY));
}
</style>
