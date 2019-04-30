<template>
  <div ref="holder" style="position: relative; z-index: 5">
    <div class="spinner "></div>
    <div class="chart hidefirst ">
      <h3 style="text-align: center">{{ title }}</h3>
      <vega-lite :spec="spec" :data="values"></vega-lite>
      <p> {{ chart }} </p>

    </div>

  </div>
</template>


<script>
import { mapState } from 'vuex'
import AugurStats from 'AugurStats'

export default {
  props: ['source', 'citeUrl', 'citeText', 'title', 'disableRollingAverage', 'alwaysByDate', 'data'],
  data() {
    return {
      values: [],
      user: null
    }
  },
  computed: {
    repo() {
      return this.$store.state.baseRepo
    },
    spec() {
      // let repo = window.AugurAPI.Repo({ githubURL: this.repo })
      console.log("data", this.data)
      if (this.data)
        this.values = this.data//this.convertKey(this.data)

      let config = {
        "$schema": "https://vega.github.io/schema/vega-lite/v2.json",
        "width": 300,
        "height": 300,
      
        // "data": {"url": "https://vega.github.io/vega-lite/data/unemployment-across-industries.json"},
        "mark": "line",
        "padding": {"left":0, "right":80, "top": 0, "bottom": 0},
        "encoding": {
          "x": {
            "timeUnit": "yearmonth", "field": "date", "type": "temporal",
            "axis": {"domain": false, "format": "%Y", "tickSize": 0}
          },
          "y": {
            // "aggregate": "sum", 
            "field": "value","type": "quantitative",
            "axis": null,
          },
          "color": {"field":"field", "type":"nominal", "scale":{"scheme": "dark2"}}
        }
        

        
      }

      
      $(this.$el).find('.showme, .hidefirst').removeClass('invis')
      $(this.$el).find('.spinner').removeClass('loader')


      return config

    }
  },
  methods: {
    renderChart() {

    },
  }
  
}

</script>
