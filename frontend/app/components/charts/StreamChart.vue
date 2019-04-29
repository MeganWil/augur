<template>
  <div ref="holder" style="position: relative; z-index: 5">
    <div class="spinner "></div>
    <div class="chart hidefirst ">
      <h3 style="text-align: center">Lines of code added by the top 10 authors visualized</h3>
      <vega-lite :spec="spec" :data="values"></vega-lite>
      <p> {{ chart }} </p>

    </div>

    <div class="form-item form-checkboxes tickradios" style="transform: translateY(-35px) !important">


          <div class="inputGroup" >
            <input id="indradio" name="comparebaseline" value="0" type="radio" v-model="user">
            <label id="front" for="indradio">Individual contributor monitoring their contributions</label>
          </div>
          <div class="inputGroup ">
            <input id="busradio"name="comparebaseline" value="1" type="radio" v-model="user">
            <label id="front" for="busradio">Business looking to adopt a project</label>
          </div>
          <div class="inputGroup ">
            <input id="comradio"name="comparebaseline" value="2" type="radio" v-model="user">
            <label id="front" for="comradio">Community Manager monitoring a repo</label>
          </div>

        
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
      let repo = window.AugurAPI.Repo({ githubURL: this.repo })
      if (this.data){
        let data = []
        Object.keys(this.data).forEach((key) => {
          data = data.concat(this.data[key])
        })
        this.values = data//this.convertKey(this.data)
      }

      let config = {
        "$schema": "https://vega.github.io/schema/vega-lite/v2.json",
        "width": 950,
        "height": 350,
      
        // "data": {"url": "https://vega.github.io/vega-lite/data/unemployment-across-industries.json"},
        "mark": "area",
        "encoding": {
          "x": {
            "timeUnit": "yearmonth", "field": "date", "type": "temporal",
            "axis": {"domain": false, "format": "%Y", "tickSize": 0}
          },
          "y": {
            "aggregate": "sum", 
            "field": "value","type": "quantitative",
            "axis": null,
            "stack": "center"
          },
          "color": {"field":"group", "type":"nominal", "scale":{"scheme": "dark2"}}
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
