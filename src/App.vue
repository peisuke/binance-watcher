<template>
  <v-app>
    <v-main>
      <v-container>
      <v-tabs grow v-model="item">
        <v-tab @click="clickBars">Bar</v-tab>
        <v-tab @click="clickGrids">History</v-tab>
        <v-tabs-items v-model="item">
          <v-tab-item eager>
            <bars 
             :title="title"
             :symbols="symbols" 
             :pairs="pairs" 
             :values="values" 
             ref='bars'
             />
          </v-tab-item>
          <v-tab-item eager>
            <grids 
             :title="title"
             :symbols="symbols" 
             :pairs="pairs" 
             :historical="historical" 
             ref='grids'
             /> 
          </v-tab-item>
        </v-tabs-items>
      </v-tabs>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios'
import Bars from './components/Bars';
import Grids from './components/Grids';
//import StackedBars from './components/StackedBars';

export default {
  name: 'App',
  components: {
    //StackedBars
    Bars,
    Grids,
  },
  data: () => ({
    item: 0,
    symbols: [],
    values: [],
    pairs: [],
    historical: [],
    title: '' 
  }),
  methods: {
    clickBars: function () {
      this.$refs.bars.$emit('update-chart')
    },
    clickGrids: function () {
      this.$refs.grids.$emit('update-chart')
    }
  },
  mounted () {
    let url = 'https://lbupa7i8jf.execute-api.us-west-2.amazonaws.com/list/GetBinanceProfitSupremeMeterSystem'
    axios.get(url).then(ret => {
      this.title = ret.data.last_modified
      this.symbols = ret.data.data.map(x => x.symbol)
      this.pairs = ret.data.data.map(x => x.pair)
      this.values = ret.data.data.map(x => x.val.slice(-1)[0])

      this.historical = []
      for (var i=0;i < ret.data.data.length; i++ ) {
        this.historical.push({data: ret.data.data[i].val, name: this.symbols[i]})
      }
    })
  }
};
</script>
